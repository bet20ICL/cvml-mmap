- static linking
	- copy library code into your program
- dynamic linking
	- declare a dependency on a shared object file
	- dynamic loader (ld.so) finds the file and loads it
	- this allows many programs to share the same library code
		- [[Question]] then surely introduces dependency hell?
		- one guy needs 1.13 but the other needs 1.20

[[Shared Object Files]]
- linux file extension .so
- contains compiled machine code used by other libraries
- Mac: `.dylib`
- Windows: `.dll`


[[GLIBC]]
- C runtime
- `printf`, `pthreads`, `malloc`

[[libstdc++]]
- C++ runtime
- `std:string`

[[Runtime]]
- 