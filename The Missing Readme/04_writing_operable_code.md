---
date: 2026-03-08
type: reference
repo: tech-book-notes
origin: curated
threads: [learning, work]
decisions: []
open_loops:
  - The chapter links to a data classes note that is an empty file, so the immutability point it was meant to carry is unwritten
  - Notes stop partway through the logging section, at INFO
---

- Operable code has built in protection, diagnostics, and controls.
- Defensive programming - safe code takes advantage of compile-time validation to prevent runtime falures.
	- Resilient code uses exception-handling best practices and handles failures gracefully
	- Avoid null values
		- especially as outputs of functions
	- Make variables immutable
		- `final` in java
		- `val` instead of `var` in scala
		- `let` in rust instead of `let mut`
		- In python, consider making [[data classes]] immutable!
	- Use type hinting and static type checkers
	- Validate inputs
		- reject bad input as early as possible!0 
- Logging
	- Use log levels (i.e. ERROR vs. INFO) to reduce volume and hint at severity
		- Trace - an extremely fine level of detail that is only turned on for specific packages or classes
			- rarely used outside of development
			- if you use traces frequently, you should use a debugger to step through code instead
		- INFO - used to tell us something about normal operation, usually milestones. Don't be greedy here