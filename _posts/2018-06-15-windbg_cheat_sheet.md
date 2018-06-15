---
layout: post
title: "WinDbg cheat sheet"
---

## Regular commands
```
~		# View all threads
~4s		# Switch to the 4th thread
~~[2668]s	# Switch to thread id 2668

k		# View native stack trace
kL		# but hide source lines
kb		# View stack trace with arguments
~2k		# View stack trace of the 2nd thread

lm		# List all the modules
lmi <module>	# Detailed info about a module
lmvm <module>	# View a module's detail

g		# Go
ld kernel32	# Load symbols for kernel32.dll
ld *		# Load symbols for all modules

x kernel32!*	# Examine and list symbols in kernel32
x kernel32!*LoadLib*	# List all the symbols in kernel32 that contain the word LoadLib

s 0 L?ffffffff c8 18 00 00	# Search for byte patterns in the memory range specified

? <expression>	# Evaluate expression

# View the definition of a specific type
dt _CLIENT_ID
dt ntdll!_RTL_CRITICAL_SECTION
dt ntdll!*	# Display all variables in ntdll
```
## Meta or dot commands
```
.sympath	Get/set path for symbol search
.cls		Clear screen
.lastevent
.detach
.expr
.reload		Reload symbol information

# Load necessary library for CLR debugging (path depends on CLR version and target architecture)
.load C:\Windows\Microsoft.NET\Framework\v2.0.50727\SOS.dll
```
## Extension commands
### Commonly used commands
```
!analyze
!handle <addr>	# Show data on the handle

!uniqstack	# View the unique stacks

!cs		# View all critical sections
!cs 1aaef4e0	# View critical section at address 1aaef4e0
!locks		# View all the locked critical sections

!address	# View all the address blocks in the process
!address 1aaef4e0	# View details about a specific memory address

!peb		# View the PEB (Process Execution Block)
!teb		# View the TEBs (Thread Execution Blocks)
!teb fe3d6000	# View a specific TEB
```
### CLR Debugging
```
!CLRStack
!Threads	# View managed threads
```
### Extensions Help
```
!exts.help	General Extensions
!Uext.help	User-Mode Extensions (non-OS specific)
!Ntsdexts.help	User-Mode Extensions (OS specific)
!Kdexts.help	Kernel-Mode Extensions
!logexts.help	Logger Extensions
!clr10\sos.help	Debugging Managed Code
!wow64exts.help	Wow64 Debugger Extensions
```

##References
http://windbg.info/doc/1-common-cmds.html
