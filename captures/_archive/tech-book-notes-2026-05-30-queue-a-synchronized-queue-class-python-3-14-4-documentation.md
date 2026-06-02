# queue — A synchronized queue class — Python 3.14.4 documentation
Source: https://docs.python.org/3/library/queue.html
Captured: 2026-05-30 | Action: reference-only

## Summary
The queue module provides synchronized queue classes for thread-safe inter-thread communication. It implements FIFO (Queue), LIFO (LifoQueue), and priority-based (PriorityQueue) queues, with SimpleQueue offering a simpler unbounded alternative. All queues handle thread synchronization internally but lack reentrancy support.

## Key Points
- Queue types: FIFO (default), LIFO, priority-based (uses heapq), and unbounded SimpleQueue
- Blocking behavior: put/get block until space/data is available (configurable via block/timeout)
- Task tracking: task_done() and join() for consumer thread coordination
- SimpleQueue: no task tracking, unbounded, C-optimized for reentrancy (e.g., in destructors)
