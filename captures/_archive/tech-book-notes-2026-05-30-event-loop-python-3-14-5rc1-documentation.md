# Event loop — Python 3.14.5rc1 documentation
Source: https://docs.python.org/3/library/asyncio-eventloop.html#asyncio.loop.run_in_executor
Captured: 2026-05-30 | Action: reference-only

## Summary
The event loop is the core of asyncio applications, managing asynchronous tasks, I/O operations, and subprocesses. Most developers should use high-level APIs like `asyncio.run()` instead of directly interacting with the loop, reserving low-level access for library/framework developers needing finer control.

## Key Points
- Event loops handle async tasks, I/O, and subprocesses; direct manipulation is discouraged for typical applications.
- Critical methods include `run_until_complete`, `run_forever`, `stop`, and thread-safe `call_soon_threadsafe`.
- Context management via `contextvars` and proper executor shutdown (`shutdown_default_executor`) are essential for reliability.
