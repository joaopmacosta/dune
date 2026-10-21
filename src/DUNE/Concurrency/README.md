# Concurrency

Threading and synchronization primitives used by the task runtime and other
concurrent code: `Thread`, atomics, mutexes/read-write locks, conditions,
barriers, semaphores, thread-local storage, `TSQueue`, scheduling, processes,
and shared memory. [`Concurrency.hpp`](../Concurrency.hpp) is the include
umbrella; the scoped lock/condition types pair resource acquisition and
release.

[`Tasks/Task.hpp`](../Tasks/Task.hpp) uses `Thread`, `TSQueue`, and TLS; the
HTTP server also consumes this package. Platform-specific implementations
live beside the interfaces, with their supported behavior expressed in the
public headers.
