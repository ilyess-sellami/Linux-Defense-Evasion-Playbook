# 04 - LD_PRELOAD & Dynamic Linker Abuse

## Overview

`LD_PRELOAD` and dynamic linker behavior in Linux can be leveraged to influence how shared libraries are loaded when executing programs. From a security perspective, this mechanism can be misused to intercept function calls, modify program behavior, or mask system activity. In defensive contexts, understanding this mechanism is important for detecting abnormal runtime library injection and runtime manipulation attempts.

---

## Linux Dynamic Linking Basics

Most Linux programs rely on **shared libraries** (`.so` files) loaded at runtime by the dynamic linker (`ld-linux.so`).

Key concepts:

* Shared libraries are loaded before program execution
* Standard libraries like `libc.so` provide core system functions
* The dynamic linker resolves function calls at runtime

---

## What is LD_PRELOAD?

`LD_PRELOAD` is an environment variable that allows a user to specify **custom shared libraries to load before any others**.

This means:

* Preloaded libraries override default system libraries
* Functions can be intercepted or replaced
* Behavior of programs can be modified at runtime

---

## Legitimate Use Cases

Although powerful, `LD_PRELOAD` is also used in legitimate scenarios:

* Debugging application behavior
* Profiling performance
* Overriding buggy library functions
* Testing and instrumentation
* Sandboxing and controlled environments

---

## Example: Basic LD_PRELOAD Usage

### 1. Create a custom shared library

```c id="9k2m9x"
#include <stdio.h>

void hello() {
    printf("Hello from custom library\n");
}
```

Compile it:

```bash id="p7d8qa"
gcc -shared -fPIC -o libcustom.so custom.c
```

---

### 2. Preload the library

```bash id="kq91dx"
LD_PRELOAD=./libcustom.so ls
```

Even though `ls` is executed, the system loads `libcustom.so` first.

---

## How LD_PRELOAD Works Internally

When a program starts:

1. The dynamic linker checks `LD_PRELOAD`
2. Loads specified shared libraries first
3. Resolves symbols (functions) in order of priority
4. Executes the program with modified library behavior

---

## Security Abuse Scenarios (Conceptual)

From a defense perspective, attackers may misuse this mechanism to:

* Intercept system calls (e.g., file access, network calls)
* Modify authentication behavior
* Hide process or file activity (via function hooking)
* Alter logging behavior at runtime
* Inject malicious logic into trusted binaries

---

## Detection Opportunities

Security teams can detect suspicious usage of `LD_PRELOAD` by monitoring:

* Environment variables in process execution
* Unexpected shared library loading
* Processes loading non-standard `.so` files
* Execution of binaries with modified runtime environments

**Example indicators:**

* `LD_PRELOAD` set in non-development environments
* Libraries loaded from `/tmp`, `/dev/shm`, or user-writable directories
* Unexpected overrides of libc functions

---

## Hardening Recommendations

* Restrict `LD_PRELOAD` in production environments
* Use secure execution policies (e.g., SELinux, AppArmor)
* Monitor dynamic linker environment variables
* Enforce trusted library paths (`/lib`, `/usr/lib`)
* Enable integrity monitoring for shared libraries

---

## Summary

`LD_PRELOAD` is a powerful Linux feature that allows runtime modification of program behavior through shared library injection. While useful for debugging and development, it can also be abused to alter program execution flow, making it an important focus area for Linux security monitoring and detection engineering.