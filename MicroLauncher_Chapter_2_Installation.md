### Index Navigation ###

This chapter is part of the [MicroLauncher Manual](MicroLauncher.md).

The previous chapter is [Chapter 1: General Information](MicroLauncher_Chapter_1_General_Information.md).

The next chapter is [Chapter 3: General Usage](MicroLauncher_Chapter_3_General_Usage.md).

# Download, Installation, First Uses #

## Download ##

The download and installation of the program is rather simple:

  * Download the source via Git
    * The Google Code Source contains the current development state of the tool

> XXX

  * Download the "golden release", the stable version

> XXX

## Installation ##

First, go in the directory:

```
 $:~/Memory$ cd microlaunch/
 
 $:~/Memory/microlaunch$ ls
 allocator           benchmark_exec.c  defines.h      dflush.h  figures     Log.h     obj        pipes.h   Signal.h  timer
 bench_descriptor.c  benchmark_exec.h  Description.c  doc       LinkList.c  main.c    Options.c  power     stride.c  toolkit.c
 bench_descriptor.h  benchmark.h       Description.h  Doxyfile  LinkList.h  main.h    Options.h  rdtsc.h   stride.h  toolkit.h
 benchmark.c         config.cfg        dflush.c       example   Log.c       Makefile  pipes.c    Signal.c  stride.s  utils
```

Second, use make:

```
 $:~/Memory/microlaunch$ make
 gcc -c bench_descriptor.c -o obj/bench_descriptor.o -O3 -Wall -Wextra -g -DX86 -DELTDOUBLE 
 gcc -c benchmark.c -o obj/benchmark.o -O3 -Wall -Wextra -g -DX86 -DELTDOUBLE 
 gcc -c benchmark_exec.c -o obj/benchmark_exec.o -O3 -Wall -Wextra -g -DX86 -DELTDOUBLE 
 benchmark_exec.c: In function ‘Benchmark_Exec’:
 benchmark_exec.c:226:14: warning: unused variable ‘diff_median’
 benchmark_exec.c:225:14: warning: unused variable ‘time_median’
 benchmark_exec.c:224:24: warning: unused variable ‘overhead_median’
 gcc -c Description.c -o obj/Description.o -O3 -Wall -Wextra -g -DX86 -DELTDOUBLE 
 gcc -c dflush.c -o obj/dflush.o -O3 -Wall -Wextra -g -DX86 -DELTDOUBLE 
 gcc -c LinkList.c -o obj/LinkList.o -O3 -Wall -Wextra -g -DX86 -DELTDOUBLE 
 gcc -c Log.c -o obj/Log.o -O3 -Wall -Wextra -g -DX86 -DELTDOUBLE 
 gcc -c main.c -o obj/main.o -O3 -Wall -Wextra -g -DX86 -DELTDOUBLE 
 gcc -c Options.c -o obj/Options.o -O3 -Wall -Wextra -g -DX86 -DELTDOUBLE 
 gcc -c pipes.c -o obj/pipes.o -O3 -Wall -Wextra -g -DX86 -DELTDOUBLE 
 gcc -c Signal.c -o obj/Signal.o -O3 -Wall -Wextra -g -DX86 -DELTDOUBLE 
 gcc -c stride.c -o obj/stride.o -O3 -Wall -Wextra -g -DX86 -DELTDOUBLE 
 gcc -c toolkit.c -o obj/toolkit.o -O3 -Wall -Wextra -g -DX86 -DELTDOUBLE 
 gcc -o microlaunch  obj/bench_descriptor.o obj/benchmark.o obj/benchmark_exec.o obj/Description.o obj/dflush.o obj/LinkList.o obj/Log.o obj/main.o obj/Options.o obj/pipes.o obj/Signal.o obj/stride.o obj/toolkit.o -O3 -Wall -Wextra -g -DX86 -DELTDOUBLE -ldl -rdynamic
 gcc timer/timer.c -o timer/timer.so -fPIC -shared -O3 -Wall -Wextra -g -DX86 -DELTDOUBLE -I.
 gcc power/timer.c -o power/timer.so -fPIC -shared -O3 -Wall -Wextra -g -DX86 -DELTDOUBLE -I.                             -D__PL_LINUX__ power/esrv/esrv_lib.so
 make -C allocator/bigarray/ all
 make[1]: Entering directory `/home/jcbeyler/Memory/microlaunch/allocator/bigarray'
 gcc bigarray.c -DELTDOUBLE -DX86 -o bigarray.so -O3 -fPIC -shared -Wall -Wextra
 make[1]: Leaving directory `/home/jcbeyler/Memory/microlaunch/allocator/bigarray'
 make -C allocator/dedicated_arrays/ all
 make[1]: Entering directory `/home/jcbeyler/Memory/microlaunch/allocator/dedicated_arrays'
 gcc sa_malloc.c -o sa_malloc.o -O3 -shared -fPIC -c -Wall -Wextra
 gcc dedicated_arrays.c sa_malloc.o -o dedicated_arrays.so -O3 -shared -fPIC -Wall -Wextra 
 make[1]: Leaving directory `/home/jcbeyler/Memory/microlaunch/allocator/dedicated_arrays'
```

## Displaying the Help ##

Now the program can run.  Without any command-line options, the software prints out the command-line option possibilities.

```
 *************************************************************************************************
 * |\   /|   '    ____  ____   ____          ____                    ____           ____  ____  *
 * | \ / |   |   |     |____| |    | |      |____|  |    |  | \  |  |      |____|  |____ |____|  *
 * |  '  |   |   |____ |   \  |____| |____  |    |  |____|  |  \ |  |____  |    |  |____ |   \  *
 *                        *
 *************************************************************************************************
 
 Usage of options for.                             /microlaunch:
 KERNEL MODE
 *************
 - Global Arguments
   --kernelname <value>: Change the kernel library to be used
   --kernelfunction <value>: Change the kernel function to be used
   --startvector <value>: Sets the vector size (in elements)
 - Optional Arguments
   --endvector <value>: Sets the end vector size (in elements)
   --stepvector <value>: Sets the step between each vector size computation
   --repetition <value>: Change the number of repetition to execute
   --metarepetition <value>: Change the number of meta-repetition to execute
   --executerepetition <value>: Change the number of Microlauncher executions to be done
   --maxalloc <value>: Change the maximum allocation (in octets)
   --cpupin <value>: Change the processor we wish to be pinned on
   --basename <value>: Add a meaningfull name for the output files.
   --maxstride <value>: Change value of the maximum stride
   --alloclib <path>: Select the allocation library
   --evalstart <value>: select evaluation start function
   --evalstop <value>: select evaluation stop function
   --evalinit <value>: select evaluation init function
   --evalclose <value>: select evaluation close function
   --evallib <value>: select evaluation library
   --nbvector <value[1,8]>: Change the maximum of allocated vectors
   --no-output: Desable the output csv file creation
   --vectsurveyor "{(start,stop,step); . . .}": Change the alignment of allocated vectors
   --vectorspacing <value>: change the space (in octet) between two consecutive allocated vector
   --omppath <value>: activates the OpenMP mode and sets the OMP library path (typically /usr/lib)
   --output-dir <value>: Change the directory where output files will be stored.
   --output-same-dir: Output files will be stored in the current directory.
   --resume: Resumes a cancelled run
   --resumeid: Changes the integer suffix for resume data files
 - Multi-process Arguments
   --kernelnames <value>: file containing the path of the benchmarks
   --nbprocess <value>: number of benchmark process you want to launch
 - Untested Arguments
   --config <value>: Select the configuration file
   --sizedummy <value>: Change the size of the dummy array
   --info <value>: Change the information display value
   --logverbosity <value>: Change the log verbosity
 
 STAND-ALONE EXECUTION MODE
 ****************************
 - Global Arguments
   --execname <value>: Change the executable file to be used
 - Optional Arguments
   --execargs <value>: Add your executable arguments (must be between quotes)
   --repetition <value>: Change the number of repetition to execute
   --metarepetition <value>: Change the number of meta-repetition to execute
   --executerepetition <value>: Change the number of Microlauncher executions to be done
 - Multi-process Arguments
   --nbprocess <value>: number of benchmark process you want to launch
```

## Executing a Kernel ##

As a second example, launch a test program in the ''example'' folder.  The name of the file is example0.c, it contains a single function named scale, which traverses an array.

To study the performance of the program, type the following command:

```
 $./microlaunch --kernelname example/example0.c --kernelfunction entryPoint
```

MicroLauncher outputs information about the run, preparing the experiment, and outputs additional information. The user should obtain the following output in order to test MicroLauncher's installation. [MicroLauncher\_Chapter\_3: General Usage|Chapter 3] presents more information about the output.

```
 *************************************************************************************************
 * |\   /|   '    ____  ____   ____          ____                    ____           ____  ____  *
 * | \ / |   |   |     |____| |    | |      |____|  |    |  | \  |  |      |____|  |____ |____| *
 * |  '  |   |   |____ |   \  |____| |____  |    |  |____|  |  \ |  |____  |    |  |____ |   \  *
 *                                              *
 *************************************************************************************************
 #15958: Warning: not able to mask the interruptions
 #15958: Compiling in Normal mode:
 gcc -fPIC -O3 -Wall -Wextra -shared -DX86 -DELTFLOAT -Wl,-soname,/tmp/microFVicWg -o /tmp/microFVicWg example/example0.c
 #15968: ===================================
 #15968: LAUNCHING BENCHMARK (KERNEL MODE)
 #15968: ===================================
 
 #15968: Launching Configuration:
 #15968:    - Meta-repetitions: 10
 #15968:    - Repetitions: 20
 - Benchmark computation process:   0% (2500000/2500000)#15968:
 Going to read dummyArray with size: 12
 - Benchmark computation process: 100% (2500000/2500000)
 #15968: Child process exiting now . . .
 #15958: Father process exiting now . . .
```

The example execution does not have super-user privileges; therefore, MicroLauncher warns it cannot mask system interruptions.  After outputs explaining the tools' execution flow, the system compiles the example.

```
 #15958: Compiling in Normal mode:
 gcc -fPIC -O3 -Wall -Wextra -shared -DX86 -DELTFLOAT -Wl,-soname,/tmp/microFVicWg -o /tmp/microFVicWg example/example0.c
```

MicroLauncher handles the allocation of arrays for kernels.  By default, it allocates a single array with 2,500,000 number of elements.

# Index Navigation #

This chapter is part of the [MicroLauncher Manual](MicroLauncher.md).

The previous chapter is [Chapter 1: General Information](MicroLauncher_Chapter_1_General_Information.md).

The next chapter is [Chapter 3: General Usage](MicroLauncher_Chapter_3_General_Usage.md).