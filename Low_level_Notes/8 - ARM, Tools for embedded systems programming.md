
### Warnings (compiler)

![[{1F801C08-943B-431C-AD1A-F31A13BB7517}.png]]![[{C092798F-0DBD-4993-843E-36F7538B2CBE}.png]]![[{B9E185C4-60DE-4EAC-AA06-F4985FC3353C}.png]]

Compiler warning:
- The warning flags can be combined.
- -Wall: only enables some warnings.
- -Wextra: More warnings.
- -Wpedantic: Catches use of extensions that does not conform to the standard.
- -Werror: Turns warnings into errors, causing compilation to fail.




### Static analyzers (compiler)

How:
- "-fanalyzer" flag.

Type of errors:
- Null pointer dereference. 
- Uninitialized variable use.
- Memory leaks.
- Use after free.
- Double free.
- Out-of-bounds access.
- Resource leaks.
- Arithmetic problems like division by 0.

Cons:
- Can miss real bugs.
- Can report false positives.
### Runtime sanitizers (compiler)

ASan - Address Sanitizer:
- "-fsanitize=address"
- Catches:
	- Out of bounds read/write
	- Use after free
	- Double free or invalid free
	- Use after return
![[{FCC01796-2A67-490C-AE80-A78800DE4647}.png]]
UBSan - Undefined Bheavior Sanitizer:
- C lets you write programs with undefined behavior.
- That can produce inconsistent results or errors.
- "-fsanitize = undefined"
- Can catch:
	- Signed integer overflow.
	- Integer divison by 0.
	- Invalid bit shifts.
	- Null pointer dereferences.
	- Misalligned memory access.
	- Som array out-of-bounds access.

LSan - Leak sanitizer:
- Reports leaked allocations, sizes and where they are allocated.
- "-fsanitize = leak"
- Detects memory allocations that werent freed and no longer reachable.
### Fuzzing (not really the compiler)

- A kind of automated testing
- Automatically feed a program millions of "random" inputs to discover bugs
- I say "random" because fuzzers can be quite intelligent in how they generate input and explore the space of possbile inputs.
- Combine fuzzing with the aforementioned sanitizers for a powerful bug finding combo
### Runtime analyzers:

Valgrind:
- Runs a program inside its own specialized environment where it can monitor, track, and check almost everything.
- Memcheck: invalid memory accesses, memory leaks, uninitialized memory ...
- Callgrind: records function calls and instruction costs.
- Find computational hotspots.
- Cachegrind: Estimates I$ and D$ behavior.

Linux Perf:
- Performance analysis tool 
- Collects performance data samples
- Other programs can visualize those samples