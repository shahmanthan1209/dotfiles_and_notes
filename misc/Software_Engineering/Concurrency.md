# References:
# [CC-\w\d+] : Clean Code - Robert C. Martin
# [SDP] : Software Design Philosophy - John Ousterhout

!! Asynchron != Parallele != Concurrent

Basic definitions:
- Bound ressources
- Mutual exclusion
- Starvation
- Deadlock : can happen if mutual exclusion + lock&wait + no preemption + circular wait
- Livelock

Execution models:
- Producer-Consumer
- Readers-Writers
- Dining Philosophers

- When multi-threading, use monitor-style locking whenever possible [SDP]
- Keep your concurrency-related code separate from other code. Get your non-threaded code working first. [CC-Chapt13]
- Take data encapsulation at heart; severly limit the access of any data that may be shared [CC-Chapt13]
    => Use copies of data / partition data in independent subsets
- Server-based locking > client-based locking [CC-AppA]
- Think about shut-down code early [CC-Chapt13]
- Test your code with quick/slow test doubles, on different platform, with varying machine load, with random tuning parameters (iterations count, threads count...)... Also force failures. [CC-Chapt13]
- If tests ever fail, track down the failure [CC-Chapt13]


/********/
// Java
/*******/
Built-in parallelism the easy way : ExecutorService

synchronized method/code blocks to handle concurrent access :
    Keep synchronized sections as small as possible
    More than one per class is a code smell !

Standard classes :
    ConcurrentHashMap > HashMap
    ReentrantLock : a lock that can be acquired in one method an released in another
    Semaphore : classic implementation, with lock count
    CountDownLatch : a lock that waits for a number of events before releasing all threads waiting on it
Full list of already existing constructs :
    http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/package-summary.html
    http://docs.guava-libraries.googlecode.com/git-history/release/javadoc/com/google/common/util/concurrent/package-summary.html

std Atomic* class references > volatile

lib Fork/Join

Example Java checkpoints (sync points):
    Thread A creates thread B
    Thread A performs a 'join' with thread B and B terminates
    Thread A exits a 'synchronize' block and thread V subsequently enters the same 'synchronize' block
    Thread A writes to a volatile variable and thread B reads it
    Placing, retrieving an object in a shared zone

Thread.stop Thread.suspend // DEPRECATED ! Do not use them

Fibers // Simple Lightweight Concurrency