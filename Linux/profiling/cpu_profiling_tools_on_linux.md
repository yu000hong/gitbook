# CPU Profiling Tools on Linux

[http://euccas.github.io/blog/20170827/cpu-profiling-tools-on-linux.html](http://euccas.github.io/blog/20170827/cpu-profiling-tools-on-linux.html)

Profiling is an effective method to provide measurements for the performance of software applications. With profiling, you get fine grained information for the components of an application, such as how often a function is called, how long a routine takes to execute and how much time are spent of different spots in the code. With these information, you could identify the performance bottlenecks and the poorly implemented parts in a software application, and find effective methods to improve them.

In this post I’ll write a brief summary of two profiling methods: `Instrumentation` and `Sampling`, and four CPU profiling tools on Linux: **perf**, **gprof**, **Valgrind** and Google’s **gperftools**.

## Profiling Methods

Different profiling methods use different ways to measure the performance of an application when it is executed. Instrumentation and Sampling are the two categories that profiling methods fall into.

### Instrumentation

Instrumentation method inserts special code at the beginning and end of each routine to record when the routine starts and ends. The time spent on calling other routines within a routine may also be recorded. The profiling result shows the actual time taken by the routine on each call.

There are two types of instrumenting profiler tools:

- source-code modifying profilers
- binary profilers

**Source-code modifying profilers** insert the instrumenting code in the source code, while the **binary profilers** insert instrumentation into an application’s executable code once it is loaded in memory.

The good thing of instrumentation method is it gives you the actual time. The inserted instrumentation code (timer calls) take some time themselves. To reduce the impact of that, at the start of each run profilers measure the overhead incurred from the instrumenting process, and later subtract this overhead from the measurement result. **But the instrumenting process could still significantly affect an application’s performance in some cases**, for example when the routine is very short and frequently called, as the inserted instrumentation would disturb the way the routine executes in the CPU.

### Sampling

Sampling measures applications without inserting any modifications. Sampling profilers record the executed instruction when the operating system interrupts the CPU at regular intervals to execute process switches, and correlates the recorded execution points with the routines and source code during the linking process. The profiling result shows the frequency with which a routine and source line is executing during the application’s run.

Sampling profilers causes **little overhead** to the application run process, and they work well on small and often-called routines. One drawback is the evaluations of time spent are statistical approximations rather than actual time. Also sampling could only tell what routine is executing currently, not where it was called from. As a result, sampling profilers can’t report call traces of an application.

## CPU Profiling Tools on Linux

### perf

The perf tool is provided by Linux kernel (2.6+) for profiling CPU and software events. You can get the tool installed by:

- Ubuntu: install **linux-tools_common**
- Debian: install **linux-base**
- Arch: install **perf-utils**
- Fedora: install *perf**

perf is based on the perf_events system, which is based on event-based sampling, and it uses CPU performance counters to profile the application. It can instrument hardware counters, static tracepoints, and dynamic tracepoints. It also provide per task, per CPU and per-workload counters, sampling on top of these and source code event annotation. It does not instrument the code, so that it has a really fast speed and generates precise results.

You can use perf to profile with `perf record` and `perf report` commands:

```
perf record -g <app> <options>
perf report
```

The perf record command collects samples and generates an output file called perf.data. This file can then be analyzed using perf report and perf annotate commands. Sampling frequency can be specified with -F option. As an example, `perf record -F 1000` means 1000 samples per second.

### gprof

GNU profiler gprof tool uses a **hybrid of instrumentation and sampling**. Instrumentation is used to collect function call information, and sampling is used to gather runtime profiling information.

Using gprof to profile your applications requires the following steps:

- Compile and link the application with `-pg` option
- Execute the application to generate a profile data file, default file name is gmon.out
- Run `gprof` command to analyze the profile data

```
g++ -pg myapp.cpp -o myapp.o
./myapp.o
gprof myapp.o
```

The gprof command prints a flat profile and a call graph on standard output. The flat profile shows how much time was spent executing directly in each function. The call graph shows which functions called which others, and how much time each function used when its subroutine calls are included. You can use the supported options [listed here](https://ftp.gnu.org/old-gnu/Manuals/gprof-2.9.1/html_mono/gprof.html#SEC4) to control gprof output styles, such as enabling line-by-line analysis and annotated source.

### Valgrind Callgrind

Valgrind is an instrumentation framework for building dynamic analysis tools. Valgrind distribution includes six production-quality tools that can detect memory issues and profile programs. Callgrind, built as an extension to Cachegrind, provides function call call-graph. A separated visualisation tool KCachegrind could also be used to visualize Callgrind’s output.

**Valgrind is a CPU emulator**. The technology behind Valgrind is `Dynamic binary instrumentation (DBI)`, whereby the analysis code is added to the original code of the client program at run-time. The profiling tool Callgrind is simulation based, it uses Valgrind as a runtime instrumentation framework. The following two papers explain how Valgrind and Callgrind work in detail.

You need use the following commands to profile your program with `valgrind`:

- Build your program as usual, no need adding any special compiler or linker flags
- Execute the program with callgrind tool to generate a profile data file, default file name is callgrind.out.<pid>
- View the generated profile data with `callgrind_annotate` or `kcachegrind` tool

```
g++ myapp.cpp -o myapp.o
valgrind --tool=callgrind myapp.o
callgrind_annotate callgrind.out.<pid>
```

### gperftools

gperftools, originally “Google Performance Tools”, is a collection of tools for analyzing and improving performance of multi-threaded applications. It offers a fast malloc, a thread-friendly heap-checker, a heap-profiler, and a cpu-profiler. gperftools was developed and tested on x86 Linux systems, and it works in its full generality only on those systems. Some of the libraries and functionality have been ported to other Unix systems and Windows.

To use the CPU profiler in gperftools, you need:

- Install the gperftools, following the instructions here
- Include gperftools header file in your application’s source files, and compile the application
- Link the library into an application with -lprofiler
- Set enrionement variable CPUPROFILE, then run the application
- Analyze the output with pprof commands

Include gperftools header files in your source file:

```c
#include "gperftools-2.6.1/src/gperftools/profiler.h"
```

Link with `-lprofiler`, profiler is in the installation directory of gperftools:

```bash
g++ -DWITHGPERFTOOLS -lprofiler -g myapp.cpp -o myapp.o
```

Set CPUPROFILE environment variable, which controls the location of profiler output data file:

```bash
export CPUPROFILE=./prof.out
```

Run pprof commands to analyze the profiling result:

```bash
pprof --text <app> ./prof.out # text output
pprof --gv <app> ./prof.out # graphical output, requires gv installed
```

