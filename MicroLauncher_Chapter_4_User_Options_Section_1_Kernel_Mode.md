### Index Navigation ###

This section is part of [Chapter 4](MicroLauncher_Chapter_4_User_Options.md) of [MicroLauncher's Manual](MicroLauncher.md).

There is no previous section.

The next section is [Section 2: Stand-Alone Mode](MicroLauncher_Chapter_4_User_Options_Section_2_Stand_Alone_Mode.md).

# Introduction #

The current page presents user options for MicroLauncher regarding the kernel mode.  Multiple groups form the different options: the kernel, vectors used by the kernel, allocation libraries, and execution process.  There are three levels of values for each option: default, configuration, and command-line.  If the command-line does not define a particular value, MicroLauncher uses the value from the configuration file.  If the configuration file does not define the value, then the default value is used.  The rest of the following page presents the options with command-line and configuration file variants.

# Kernel #

## Kernel's File ##

The kernelname option defines the kernel file to use.  The file can be in assembly, C-code, object, or dynamic library form.  MicroLauncher automatically compiles the code into a dynamic library and loads it at run-time.

Example:

**Command-Line**
```
 ./microlaunch --kernelname test.s
```
**Configuration File**
```
 <kernelName value="test.s" />
```

If the kernelname is a directory, MicroLauncher separately executes each file of the directory, except hidden files.

Example:

**Command-Line**
```
 ./microlaunch --kernelname ../microcreator/output
```

**Configuration File**
```
<kernelName value="../microcreator/output" />
```
## Kernel's Function ##

In kernel mode, MicroLauncher needs to know what function to call.  The kernelfunction option handles the function name.  By default, the name of the function is _entryPoint_.  If in the test.s file, there is a function named foo, MicroLauncher calls it with the following arguments:

**Command-Line**
```
 ./microlaunch --kernelname test.s --kernelfunction foo
```

**Configuration File**
```
<kernelFunction value="foo" />
```

# Vectors #

Historically, MicroLauncher was a launcher for kernels only traversing arrays.  Currently, the tool mixes the notion of vector size with a number passed to the kernel's entry function.  The tool allocates the arrays, initializes them, and then passes them to the benchmark program.  Though 99% of benchmark programs have arrays, MicroLauncher does allow the execution of a kernel program without any vectors pre-allocated.  The following section presents the options that handle vectors.

## Number of Vectors ##

The option nbvector defines the number of vectors.  The values authorized are between 0 and 8.  By default, the value is 1.

**Command-Line**
```
  ./microlaunch --kernelname example/example0.c --nbvector 5
```

**Configuration File**
```
 <nbVector value="5" />
```
## Vector Size ##

### Same Size ###

There are two solutions to define the size of the allocated vectors.  The first solution is to use the same size for each vector using the options startvector, endvector, and stepvector.  If the user does not define endvector, MicroLauncher assumes it to be the value of startvector.  If the user also does not define the startvector variable, the tool sets both to 2500000.

#### Start Vector ####

**Command-Line**
```
  ./microlaunch --kernelname example/example0.c --nbvector 5 --startvector 1000
```
**Configuration File**
```
 <startVector value="1000" />
```
The previous example creates five arrays.  Each array has a thousand elements.  An element can be a float, a double, or anything the user defines using the [data size option](MicroLauncher_Chapter_4_User_Options_Section_1_Kernel_Mode#Data_Size.md).

#### End Vector ####

Defining endvector allows MicroLauncher to test various sizes.  In order to test every size between 1000 and 2000, the user can do:

**Command-Line**
```
  ./microlaunch --kernelname example/example0.c --nbvector 5 --startvector 1000 --endvector 2000
```
**Configuration File**
```
 <endVector value="2000" />
```
#### Step Vector ####

Finally, if the user wishes to test every other value, stepvector permits it.  By default, its value is 1.

**Command-Line**
```
  ./microlaunch --kernelname example/example0.c --nbvector 5 --startvector 1000 --endvector 2000 -stepvector 2
```
**Configuration File**
```
 <stepVector value="2" />
```
### Different Sizes ###

In particular cases, the user might wish to define different sizes for each vector.  In such cases, the use of the nbsizes option is necessary.

**Command-Line**
```
  ./microlaunch --kernelname example/example0.c --nbvector 5 --nbsizes="{100,2000,500,100,50,2000}"
```
**Configuration File**
```
 <nbSizes value="{100,2000,500,100,50,2000}" />
```

The option requires accolades and quotes.  If the user also defines nbvector, the number of elements in nbsizes must be the same.

## Data Size ##

The size of each element is defined by **--data-size**.  It can be a numerical value, float, or double.  By default, it is set to float.

**Command-Line**
```
 ./microlaunch --kernelname example/example0.c --datasize 42
 ./microlaunch --kernelname example/example0.c --datasize float
 ./microlaunch --kernelname example/example0.c --datasize double
```
**Configuration File**
```
 <dataSize value="42" />
 <dataSize value="float" />
 <dataSize value="double" />
```
## Initializing the Data ##

Some users wish to be able to modify the initialization of the array.  Currently, if the data are floating point elements, MicroLauncher initializes it 1.0.  Otherwise, the array elements are set to 0.  The function must be contained in the kernel file.

**Command-Line**
```
./microlaunch --kernelname example/example2.c --nbvector 5 --nbsizes="{100,20,30,50,40}" --initfunction myInit --data-size 42
```

**Configuration File**
```
<initFunction value="myInit" />
```
## Vector Surveyor ##

In some benchmarks, the user may want to have a special alignment set for the allocated vectors.  It can be done by the **--vectsurveyor** argument.  The user must give three values for each specified vector.  The first one is the base alignment, the second is the last alignment, and the third is the increment between each alignment set.  Here is a typical command-line for this argument:

**Command-Line**
```
 ./microlaunch --kernelname example/example0.c --nbvector=3 --vectsurveyor="{(0,256,16);(0,256,16);(0,256,16);}"
```
**Configuration File**
```
<vectSurveyor value="{(0,256,16);(0,256,16);(0,256,16);}" />
```

The previous example performs sixteen different alignments for each vector; therefore, performing 16 **16** 16 configurations.

## Maximum Stride ##

Certain users wish to have a code, which does not traverse each element but actually every other element or even less.  In order for the computation to be correct and not risk accessing data past the array, the use of maxstride is necessary.  It divides the size of the array with the value provided before calling the kernel function.  For example, assuming a size of 1,000 with a stride of 2, MicroLauncher provides the number of iterations requested as 500.

**Command-Line**
```
./microlaunch --kernelname example/example0.c --maxstride 25
```

**Configuration File**
```
<maxStride value="25" />
```

By default, the size of the array is 2,500,000, it actually only requests 1,000,000 iterations.

## Iteration Count ##

It is sometimes necessary to create big arrays and only iterate on a certain number of elements.

**Command-Line**
```
./microlaunch --kernelname example/example0.c --iteration-count 50
```

**Configuration File**
```
<iterationCount value="50" />
```

The vector size is by default 2,500,000, the execution, with such an option, allocates an array of 2,500,000 elements but only requests a traversal of 50.

# Allocation Library #

The current section presents the allocation library.  In kernel mode, MicroLauncher allocates arrays for the microbenchmark.  The user may redefine the allocation process.  By default, the allocation aligns the arrays on a page.  As with the [evaluation library](MicroLauncher_Chapter_4_User_Options_Section_1_Kernel_Mode#Evaluation_Library.md), the user may redefine the allocation function via the command-line option alloclib.  The option takes a dynamic library containing a certain number of functions:

  * allocationInit: the function to initialize the library
```
int allocationInit (const Description *description);
```

  * allocationMalloc: the actual allocation function
```
void* allocationMalloc (size_t size);
```

  * allocationFree: the function to free allocations
```
void allocationFree (void *ptr);
```

  * allocationClose: the function to close the library
```
void allocationClose (void);
```

As an example:

**Command-Line**
```
./microlaunch --kernelname example/example0.c --alloclib allocator/dedicated_arrays/dedicated_arrays.so
```

**Configuration File**
```
<allocLib value="allocator/dedicated_arrays/dedicated_arrays.so" />
```

# Evaluation Library #

The evaluation library of MicroLauncher provides a metric when launching microbenchmark programs.  The user may redefine the evaluation library.  As for the [allocation library](MicroLauncher_Chapter_4_User_Options_Section_1_Kernel_Mode#Allocation_Library.md), the command-line option evallib redefines the evaluation library.  The functions have predefined names the user must respect:

  * evaluationInit: initialize the evaluation library and returns a context for following function calls
```
void *evaluationInit (void);
```

  * evaluationStart: start the metric
```
 double evaluationStart (void *context);
```

  * evaluationStop: stop the metric
```
 double evaluationStop (void *context);
```

  * evaluationClose: close the evaluation library, returns **EXIT\_SUCCESS** if there were no errors
```
 int evaluationClose (void *context);
```
MicroLauncher performs the subtraction between the value provided by the stop and the start.  The tool provides two metric libraries.  The first handles performance using the rdtsc register, the second gives an energy consumption or power usage metric.  By default, the tool uses the performance metric.  To illustrate the option and use the provided power metric:

**Command-Line**
```
 ./microlauncher --kernelname example/example0.c --evallib power/timer.so
```

**Configuration File**
```
 <evalLib value="power/timer.so" />
```

# Repetitions #

Repetitions are a core element in MicroLauncher's methodology.  Calculating metrics for multiple runs and comparing the results provides a means to verify data stability.  For the kernel mode, the tool provides three levels of repetitions: execution, meta, and base.  The general algorithm for the repetitions is:

```
 for each execution repetition
   Re-launch Microlauncher
   Allocate the vectors
   for each meta repetition
     Start the metric
     for each base repetition
       run the microbenchmark program
     Stop the metric
     Provide (start - stop) to user
```

The rest of the section provides more information about base repetitions.  [The common option section](MicroLauncher_Chapter_4_User_Options_Section_2_Stand_Alone_Mode#Repetitions.md) presents the other two: repetitions and metarepetitions.

## Base Repetition ##

The base repetitions allow the user to request multiple executions of the kernel program between the metric start and stop.  Generally, the base repetition's purpose is to lengthen the benchmark program for stability reasons.

When using a small number of repetitions:

**Command-Line**
```
 ./microlaunch --kernelname example/example0.c --metarepetition 2 --repetition 1
```

The file kernel\_default\_2500000.csv contains:

```
 2.8688,1,0,1,0,
 5.0437,1,0,1,0,
```

However, when augmenting the number of repetitions to 3,000:

**Command-Line**
```
 ./microlaunch --kernelname example/example0.c --metarepetition 2 --repetition 3000
```

The tool provides the following results:

```
 2.9336,1,0,1,0,
 2.9292,1,0,1,0,
```

**Configuration File**
```
 <metaRepetition value="2" />
 <repetition value="3000" />
```

# Execution #

The following options relate to anything modifying the execution of MicroLauncher.

## CPU Pinning ##

CPU pinning allows the choice of which core to execute MicroLauncher.

**Command-Line**
```
 ./microlaunch --kernelname example/example0.c --cpupin 5
```

**Configuration File**
```
 <cpuPin value="5" />
```

N.B.: the option is only valid if the option [nbprocess](MicroLauncher_Chapter_4_User_Options_Section_3_Common_Options#Number_of_Processes.md) is not used

## Base Name ##

The base name redefines the output file's name.  By default, the output file name contains the keyword default.  The option modifies it into the choice of the user.  Consider the following execution:

**Command-Line**
```
 ./microlaunch --kernelname example/example0.c
```

After the execution, the program generates an output file named kernel\_default\_2500000.csv.

**Command-Line**
```
 ./microlaunch --kernelname example/example0.c --basename test
```

**Configuration File**
```
 <baseName value="test" />
```

By default, kernel\_test\_2500000.csv is the output file.

However, the basename option is ignored if the kernelname option is a folder.  In the case of a folder, the basename is replaced by the current filename.

## Output Directory ##

By default, MicroLauncher creates the output files in the output directory.  The user may modify the behavior with the --output-dir directory.  If the directory does not exist, the tool creates it.

In certain cases, the user might not want a directory for the output files, in which case the user adds the --output-same-dir option.

**Command-Line**
```
 ./microlaunch --kernelname example/example0.c --output-same-dir
 ./microlaunch --kernelname example/example0.c --output-dir test
```

**Configuration File**
```
 <outputSameDir />
 <outputDir value="test" />
```

## OpenMP ##

OpenMP is a parallel language extension paradigm.  To enable the use of OpenMP in a microbenchmark program, the user must provide the path to the library using the --omppath option.

**Command-Line**
```
./microlaunch --kernelname example/example0.c --omppath /usr/lib/x86_64-linux-gnu
```

**Configuration File**
```
 <ompPath value="/usr/lib/x86_64-linux-gnu" />
```
## Resume ##

If MicroLauncher stopped or crashed for any reason, it is possible to resume its execution at the last configuration.  The tool recalculates any results during intermediate repetition or metarepetition in order to provide stability of the results.

An example is:

```
 ./microlaunch --kernelname example/example0.c --startvector 20000 --stopvector 20200
```

If the tool halted in the middle of the execution, executing the following resumes the program at the last vector size:

```
 ./microlaunch --resume
```

## No Output ##

If the user is not interested in the output from MicroLauncher, the option --no-output is used.

**Command-Line**
```
 ./microlaunch --kernelname example/example0.c --no-output
```

**Configuration File**
```
 <noOutput />
```

## Dummy Vector Size ##

Between metarepetitions, a dummy vector traversal flushes the caches.  A dummy vector in the tool is an array not used during the kernel's execution.  The --sizedummy option modifies the dummy vector size.

**Command-Line**
```
 ./microlaunch --kernelname example/example0.c --sizedummy 100000
```

**Configuration File**
```
 <sizeDummy value="100000" />
```

## Information Output ##

When considering a kernel program, the metric measures each metarepetition.  However, generally MicroLauncher executes kernel functions containing a single loop.  In such cases, the user wishes to know the number of cycles per iteration.  In other cases, it is the metric for the whole function or for the whole meta-repetition.

The --info argument accepts string values:

  * iteration: the metric provided is per iteration, repetition, and meta-repetition

  * function: the metric provided is per repetition and meta-repetition

  * raw: the metric provided is per meta-repetition

By default, the information value is per iteration.

```
 ./microlaunch --kernelname example/example0.c
 
 $:~/Memory/microlaunch$ head -n 2 output/kernel_default_2500000.csv 
 Cycles per iteration,Id of current run,Number of resumes,Number of arrays,Vector #1 alignment,
 3.1140,1,0,1,0,
```

With raw values:

**Command-Line**
```
 ./microlaunch --kernelname example/example0.c --info raw
```

**Configuration File**
```
 <info value="raw" />
```

```
 $:~/Memory/microlaunch$ head -n 2 output/kernel_default_2500000.csv 
 Cycles per iteration,Id of current run,Number of resumes,Number of arrays,Vector #1 alignment,
 163671142.0000,1,0,1,0,
```

## Output Log ##

By default, MicroLauncher logs information directly to the screen.  If required, the tool redirects the output to a given file.  The option to use is --log-output:

**Command-Line**
```
 ./microlaunch --kernelname example/example0.c --info raw --log-output test
```

**Configuration File**
```
 <logOutput value="test" />
```

### Verbosity ###

To reduce the amount of logging, the option --logverbosity modifies the verbosity.  The higher the value, the less the tool logs.  0 is the default value.

**Command-Line**
```
 ./microlaunch --kernelname example/example0.c --logverbosity 3
```

**Configuration File**
```
 <logVerbosity value="3" />
```

# Index Navigation #

This section is part of [Chapter 4](MicroLauncher_Chapter_4_User_Options.md) of [MicroLauncher's Manual](MicroLauncher.md).

There is no previous section.

The next section is [Section 2: Stand-Alone Mode](MicroLauncher_Chapter_4_User_Options_Section_2_Stand_Alone_Mode.md).