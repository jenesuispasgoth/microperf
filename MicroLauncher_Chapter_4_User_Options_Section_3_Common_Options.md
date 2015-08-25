### Index Navigation ###

This section is part of [Chapter 4](MicroLauncher_Chapter_4_User_Options.md) of [MicroLauncher's Manual](MicroLauncher.md).

The previous section is [Section 2: Stand-Alone Mode](MicroLauncher_Chapter_4_User_Options_Section_2_Stand_Alone_Mode.md).

There is no next section.

# Introduction #

MicroLauncher presents two modes of execution: kernel and stand-alone. The following page presents user options for MicroLauncher regarding common options to both modes.

# Repetitions #

MicroLauncher is a tool providing metric measurements. In order to provide stable numbers, experimental repetitions provide statistical mean, median, minimum, and maximum values.

## Execution Repetition ##

The execution repetition determines whether the results are stable from one run of MicroLaunch to the next. If the metric is unstable, different allocations might have an impact to the benchmark program. One obvious problematic factor is data alignment. The command-line option handling the execution repetition is executerepetition:

**Command-Line**
```
 ./microlaunch --kernelname example/example0.c --executerepetition 2
```

**Configuration File**
```
 <executeRepetition value="2" />
```

When using the execution repetition option, the tool generates separate files. In this case, the files are named:

```
 $:~/Memory/microlaunch$ ls output/
 kernel_execution_0__default_2500000.csv  kernel_execution_1__default_2500000.csv
```

## Meta Repetition ##

The meta repetitions surround the actual metric calculation and repeats them. The reason for the metric repetition is to evaluate the stability of the results with a given allocation. If the results are not stable, the data size might be an issue. Indeed, if the size is close to a cache level size boundary, the results become unstable. Another possibility is the number of base repetitions. If the number is too low, the measure can also be subject to environmental noise.

**Command-Line**
```
 ./microlaunch --kernelname example/example0.c --metarepetition 2
```

**Configuration File**
```
 <metaRepetition value="2" />
```

When using the metarepetition option, the output file contains one line per metarepetition. Comparing the values provides information on the stability of the results. With the previous execution, the file kernel\_default\_2500000.csv contains:

```
 Cycles per iteration,Id of current run,Number of resumes,Number of arrays,Vector #1 alignment,
 3.0395,1,0,1,0,
 2.9367,1,0,1,0,
```

In the previous example, the results are almost equal. Therefore, the user can consider the experiment stable.

## Configuration File ##

With over forty options, the option config allows users to define their own default values to each command-line option. The configuration file is an XML file and each option. The root node of the configuration file is microlaunch and must contain a config node. Then, each option is defined by a particular node and explained in the current chapter with each option. As an example, consider the following file:

```
 <?xml version="1.0" encoding="UTF-8"?>
 
 <microlaunch>
    <config>
        <metaRepetition value="100" />
        <repetition value="40" />
        <executeRepetition value="2" />
        <baseName value="xml_test" />
        <execName value="ls" />
        <execArgs value="-tla" />
        <suppressOutput />
    </config>
 </microlaunch>
```

A configuration file is pratical for more advanced users. Once the methodology and option choosing done, the configuration file allows a user to no longer use the command-line options but simply provide a given xml file.

# Multi-Process #

HPC codes often execute code on every core of the architecture. The program performs the same calculations or memory transfers on each core. MicroLauncher simulates the effect by launching the same program or microbenchmark on each core of the machine. However, users sometimes wish to see the behavior of a given code on one, two, four, or eight cores. The following options handle the number of cores used and how the output is handled.

## Number of Processes ##

The option **nbprocess** handles the number of cores used for the experiment. MicroLauncher mixes the number of processes and the number of cores since it pins each process on a different core. For example, requesting four processes implies MicroLauncher pins each process on a different core. In cases where the user requests more processes than available on the machine, MicroLauncher exits with an error message.

```
 ./microlaunch --config config_exec.xml --execname example/stand-alone-sleep.c --nbprocess 4
```

## Each Metric Outputs Data ##

The user might wish to obtain the metric output of each process in order to check correctness or each output. The option all-metric-output permits the behavior by creating a separate file per core.

# Index Navigation #

This section is part of [Chapter 4](MicroLauncher_Chapter_4_User_Options.md) of [MicroLauncher's Manual](MicroLauncher.md).

The previous section is [Section 2: Stand-Alone Mode](MicroLauncher_Chapter_4_User_Options_Section_2_Stand_Alone_Mode.md).

There is no next section.