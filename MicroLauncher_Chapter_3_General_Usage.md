### Index Navigation ###

This chapter is part of the [MicroLauncher Manual](MicroLauncher.md).

The previous chapter is [Chapter 2: Installation and First Use](MicroLauncher_Chapter_2_Installation.md).

The next chapter is [Chapter 4: User Options](MicroLauncher_Chapter_4_User_Options.md).

# Introduction #

MicroLauncher is a tool to execute benchmark programs in a closed and stable environment.  Providing the user with more than forty options to fine-tune the experimental runs, the tool requires a manual presenting each possibility.  The current chapter presents the tool via the input files used, the output it generates and how it links to [MicroCreator](MicroCreator.md).  As presented in [MicroLauncher\_Chapter\_2: Installation and First Use Chapter 2], the manual assumes the user has installed the golden version of the tool.

# Input and Output #

As any tool, MicroLauncher uses input data to provide an output.  In the case of MicroLauncher, the input is a program, whether stand-alone or a kernel function.  The output is a Comma Separated Values (CSV) file with the performed measurements.

## Input ##

The tool accepts a variety of input file formats:

  * Assembly

  * C-code

  * Object code

  * Dynamic library

In any other case than the dynamic library, the tool compiles the input into a temporary dynamic library and uses it as the input.  The input file must be compiler-accepted code. Otherwise, the subsequent compilation fails.  In the input file, the user must define an entry point function MicroLauncher calls at each experimental measurement.  By default, ''entryPoint'' is the function's name but the user may modify it via a command-line option.  Once the benchmark program compiled, MicroLauncher executes a number of runs and uses the defined metric, which is by default performance.  The numbers of runs are also modifiable via the options.

### Getting Stable Results ###

In order to get stable results, MicroLauncher performs a certain number of tasks and the user must also handle a few options correctly.  The summarized algorithm for MicroLauncher's execution is:

```
 Allocate the arrays
 
 for the number of metarepetitions
    Calculate the overhead
    Flush the caches
    Run the experiment a number of times
    Subtract overhead from experiment metric evaluation
    Add result to list
```

The allocation library manages array allocation.  By default, the library aligns the data to a page size but the user may redefine the behavior of the allocator.  The metarepetitions is an option provided to the tool and helps the user determine the stability of the results.  For example, if the user obtains numbers such as five cycles and twenty cycles for the same program, there is clearly a problem with the methodology.  [MicroLauncher\_Chapter\_6: Tips and Tricks Chapter 6] presents insight on what the user should do in such cases.

## Output ##

The output file of MicroLauncher is a Comma Separated Values (CSV) file.  The file contains a number of lines, each representing an iteration of the experiment repetition.  ''Metarepetition'' is the repetition's name in the tool's terminology and users may modify it via a command-line option.

An example of the file's output is:
```
 Cycles per iteration,Id of current run,Number of resumes,Number of arrays,Vector #1 alignment,
 3.2908,1,0,1,0,
 3.1223,1,0,1,0,
 3.0230,1,0,1,0,
```
In the example, the program provides number of cycles per iteration.  The experiment was performed three times therefore the output contains three lines. The other file columns are the identifier for the run, the number of resumes, the number of allocated arrays, and their alignment. MicroLauncher can fail an execution and resume its execution. The identifier and resume columns provide information on the run number and how many resumes have happenned. Finally, MicroLauncher allocates arrays for the executed kernel. The two last columns provide information on the number of arrays allocated and the alignments used.

### Metric ###

MicroLauncher uses metric libraries to provide feedback to the user.  By default, the performance metric is the number of cycles per iteration.  For any type of metric, the tool performs the same actions.  A call to the metric is done before the benchmark program and after.  As options for the metric calculation, there are three possibilities:

  * Raw values: MicroLauncher does nothing; the metric values provided by the metric library are used

  * Function cost: the tool returns the metric per function call, thus dividing the total raw cost by the number of repetitions

  * Iteration cost: MicroLauncher returns the metric from the Function cost but divides it by the iteration count, which is defined as the return of the evaluated function

[Chapter 4](MicroLauncher_Chapter_4_User_Options#Metrics.md) explains the different metrics possible.

# Examples of Usage #

Often, the easiest way to understand a tool is to consider examples.  The following section presents two examples; one using a stand-alone execution and another using a microbenchmark program.

## Microbenchmark Program ##

In the example folder, there is a file named example0.c.  The file contains a simple function, which traverses through an array:
```
 /*
 Copyright (C) 2011 Exascale Research Center
 
 This program is free software; you can redistribute it and/or
 modify it under the terms of the GNU General Public License
 as published by the Free Software Foundation; either version 2
 of the License, or (at your option) any later version.
 
 This program is distributed in the hope that it will be useful,
 but WITHOUT ANY WARRANTY; without even the implied warranty of
 MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
 GNU General Public License for more details.
 
 You should have received a copy of the GNU General Public License
 along with this program; if not, write to the Free Software
 Foundation, Inc., 51 Franklin Street, Fifth Floor, Boston, MA  02110-1301, USA.
 */
 
 #include <stdint.h>
 #include <stdio.h>
 
 unsigned int entryPoint (unsigned int n, void *tab, unsigned int elemSize)
 {
     double *ptr = tab;
     unsigned int i;
     double sum = 5;
 
     for (i=0; i < n; i ++)
     {
         sum += ptr[i];
     }
 
     tab[0] = sum;
      
     (void) elemSize;
     return i;
 }
```

The function returns the variable ''i'' because it represents the number of iterations.  MicroLauncher uses the value to calculate the metric per loop iteration.  Here is the output of the tool with the example file:

```
 $:~/microlaunch$ ./microlaunch --kernelname example/example0.c 
 *************************************************************************************************
 *  \   /    '    ____  ____   ____          ____                    ____           ____  ____   *
 *   \ /                ____                 ____             \             ____    ____  ____   *
 *    '           ____     \   ____   ____           ____      \     ____           ____     \   *
 *                                                                                               *
 *************************************************************************************************
 #22328: Warning: not able to mask the interruptions
 #22328: Compiling in Normal mode :
 gcc -fPIC -O3 -Wall -Wextra -shared -Wl,-soname,/tmp/microPw0OLa -o /tmp/microPw0OLa example/example0.c
 #22338: ===================================
 #22338: LAUNCHING BENCHMARK (KERNEL MODE)
 #22338: ===================================
 #22338: Launching Configuration :
 #22338:         - Meta-repetitions : 10
 #22338:         - Repetitions : 20
 - Benchmark computation process : 100% (2500000/2500000)
 #22338: Child process exiting now...
 #22328: Father process exiting now...
```

By default, MicroLauncher uses 10 as the value for the metarepetitions, 20 for the repetitions and 2,500,000 for the size of the array.  By default, the output file's name is output/output\_kernel\_default\_2500000.csv.  The naming convention is output_<kernel execution>_<default or defined via the basename option>_<size of the first array>.csv. In the case of the example, the output file contains:_

```
 Cycles per iteration,Id of current run,Number of resumes,Number of arrays,Vector #1 alignment,
 4.3548,1,0,1,0,
 4.3875,1,0,1,0,
 4.3377,1,0,1,0,
 4.4592,1,0,1,0,
 4.5070,1,0,1,0,
 4.3300,1,0,1,0,
 4.0335,1,0,1,0,
 4.2221,1,0,1,0,
 4.1086,1,0,1,0,
 4.1229,1,0,1,0,
```

The file contains ten lines since it is the number of metarepetitions.  [MicroLauncher\_Chapter\_4: User Options Chapter 4] explains each option to fine tune the use of the tool.

## Stand-Alone Program ##

As a stand-alone example, MicroLauncher contains in the example folder a program named example/stand-alone-sleep.c.  The file is a simple program sleeping for a second and returning:
```
 /*
 Copyright (C) 2011 Exascale Research Center
 
 This program is free software; you can redistribute it and/or
 modify it under the terms of the GNU General Public License
 as published by the Free Software Foundation; either version 2
 of the License, or (at your option) any later version.
 
 This program is distributed in the hope that it will be useful,
 but WITHOUT ANY WARRANTY; without even the implied warranty of
 MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
 GNU General Public License for more details.
 
 You should have received a copy of the GNU General Public License
 along with this program; if not, write to the Free Software
 Foundation, Inc., 51 Franklin Street, Fifth Floor, Boston, MA  02110-1301, USA.
 */
 
 #include <unistd.h>
 
 int 
 main (void)
 {
        sleep (1);
        return 0;
 }
```
To use MicroLauncher with a stand-alone program, the user must compile the program and then use the execname option:

```
 $:~/microlaunch$ gcc -O3 -o example/test example/stand-alone-sleep.c 
 $:~/microlaunch$ ./microlaunch --execname example/test 
 *************************************************************************************************
 *  \   /    '    ____  ____   ____          ____                    ____           ____  ____   *
 *   \ /                ____                 ____             \             ____    ____  ____   *
 *    '           ____     \   ____   ____           ____      \     ____           ____     \   *
 *                                                                                               *
 *************************************************************************************************
 #22802: Warning: not able to mask the interruptions
 #22806: ===================================
 #22806: LAUNCHING BENCHMARK (EXEC MODE)
 #22806: ===================================
  - Overhead computation process : 100% (10/10)
  - Benchmark computation process : 100% (10/10)
  - Data saving... 100%
 #22806: Child process exiting now...
 #22802: Father process exiting now...
```

By default, the system executes the program ten times and the output file is output/output\_exec\_default.csv.  The naming convention for the output file is output\_exec_<default or the value of the basename option>.csv.  In the case of the example, the file contains:_

```
 $:~/microlaunch$ cat output/output_exec_default.csv
 Overhead,Executable,Executable - Overhead
 2238514,2529793472,2527554958,
 2230818,2530172515,2527941697,
 2279164,2528942078,2526662914,
 2419860,2529778594,2527358734,
 2239360,2529957884,2527718524,
 2249048,2530861902,2528612854,
 2217092,2530347905,2528130813,
 888692,2528979696,2528091004,
 887724,2530140964,2529253240,

 ,Average,Min,Max,Median,
 Overhead,1995050.80000000,887724,2419860,2239360,
 Executable,2529796421.40000010,2528942078,2530861902,2529957884,
 Executable-Overhead,2527801370.59999990,2526772112,2528442042,2527893351
```
In the case of the stand-alone execution, MicroLauncher outputs the overhead of executing an empty program, the metric for executing the target application and the difference between the two.  At the end of the file are three lines for the average, minimum, maximum, and median for each value type.

# Linking with MicroCreator #

MicroLauncher was originally destined to handle directly benchmark programs from the [MicroCreator tool](MicroCreator.md).  By default, MicroCreator now creates assembly files or C-code files.  The only requirement for MicroLauncher to provide meaningful results is for the benchmarks to return the number of iterations if the user wishes to have a metric based per iteration.  Though now the tool allows more possibilities than the original version, MicroLauncher continuously maintains MicroCreator generated programs.

# Conclusion #

In conclusion, the chapter shows how to use MicroLauncher in the general case.  The various sections explain certain options but there are many various options explained in the next chapter with examples.  The important element to keep in mind is, MicroLauncher is a launcher but the user must understand a minimum how it calls a benchmark program and what it must return for a correct output by the tool.  [Chapter 6](MicroLauncher_Chapter_6_Tips_Tricks.md) provides a couple of tips and tricks to stabilize the user's results.

# Index Navigation #

This chapter is part of the [MicroLauncher Manual](MicroLauncher.md).

The previous chapter is [Chapter 2: Installation and First Use](MicroLauncher_Chapter_2_Installation.md).

The next chapter is [Chapter 4: User Options](MicroLauncher_Chapter_4_User_Options.md).