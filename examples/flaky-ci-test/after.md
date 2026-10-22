# Why the cart test fails on CI about 1 run in 14

**The cart test fails because test files that run at the same time share one
Redis database, and the session tests wipe that whole database after each
test.** Giving each test worker its own database stopped the failure in local
repeats, so that is the fix I recommend.

## What fails and how often

The cart test "keeps items after login" sometimes finds an empty cart where it
expects two items.

It failed in 11 of the last 150 CI runs on main, about 1 in 14. All 11 failures
came after the change that made CI run four test workers at once instead of
two. In the 120 runs before that change, it did not fail once.

A test worker is one of the parallel threads Vitest, our test runner, uses to
run several test files at the same time.

## Why it happens

Two test files touch the same data at the same moment, and one of them deletes
it. Redis is the in-memory store that holds carts and sessions, and every test
worker uses the same Redis database. A failing run goes like this:

1. Worker 1 runs the cart test and adds two items to the cart.
2. While it is still adding them, worker 2 finishes a session test. The session
   tests' clean-up step, which runs after every test, wipes the entire database
   rather than only the keys that test created.
3. Worker 1 reads the cart back, finds it empty, and the test fails.

With two workers this overlap never showed up in 120 runs. With four, more
files run side by side, so the wipe lands in the middle of the cart test more
often.

## How I confirmed it

I ran the cart and session test files together, over and over, on my laptop.
The failure comes back with four workers and goes away when the workers stop
sharing a database.

| Setup | Repeats | Failures |
| --- | --- | --- |
| Four workers, as on CI | 50 | 4 |
| One worker | 50 | 0 |
| Four workers, each with its own database | 200 | 0 |

All repeats ran on a laptop, not on the CI runner. I did not look into other
possible causes, such as network delays or a different Redis version on CI.

## What I recommend

**Give each test worker its own Redis database.** One Redis server can hold
several numbered databases, and today every worker uses database 0. Vitest
tells each worker its own number, so the test setup can pick its database from
that number and the workers can no longer wipe each other's data.

| Option | What changes | What we know so far |
| --- | --- | --- |
| Each worker gets its own database (recommended) | The test setup picks the database from the worker's number | No failures in 200 local repeats. The effect on CI's failure rate is TBD, to check after a week on main. |
| Go back to fewer workers | CI runs one or two workers again | No failures with one worker in 50 local repeats. How much slower CI gets is not measured. |
| Clean up only the test's own keys | The clean-up step deletes keys with the test's prefix instead of the whole database | A second guard on top of the first option. Not tried yet. |
| Retry a failed test twice | CI reruns a failing test up to two times | A stopgap only: it reruns the failure instead of removing its cause. |

## Still open

The session test "expires after TTL" (TTL is how long a session lives before it
expires) also failed in 3 of the same 150 runs. It may have the same cause, but
I have not reproduced it. If it keeps failing after the fix above, it needs its
own look.

## Notes

<details>
<summary>Code locations, error message and commands</summary>

- The whole-database wipe: `tests/helpers/redis.ts:23`, `redis.flushdb()` in
  `afterEach`. It is not limited to a key prefix.
- Worker count: `vitest.config.ts:9`, `maxThreads` changed from 2 to 4 in #482.
- Where the cart reads back empty: `src/cart/store.ts:41`, `getItems()` returns
  `[]` after `cartStore.add()` was interrupted.
- Failure message: `expected [] to have length 2`.
- Reproduction loop (50 repeats, four workers):

  ```bash
  for i in $(seq 50); do
    npx vitest run tests/cart.test.ts tests/session.test.ts \
      --poolOptions.threads.maxThreads=4
  done
  ```

- One worker: the same loop with `maxThreads=1`.
- Own database per worker: `db: Number(process.env.VITEST_POOL_ID)` in the
  test Redis client options.
- Prefix-only clean-up: `SCAN` for the test's prefix, then `DEL`, in place of
  `FLUSHDB`.
- Retry stopgap: `retry: 2` in the Vitest config.

</details>
