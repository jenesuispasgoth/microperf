### Index Navigation ###

This section is part of [Chapter 7: MicroLauncher's Internals](MicroLauncher_Chapter_7_MicroLauncher_Internals.md) of [MicroLauncher's Manual](MicroLauncher.md).

There is no previous section.

There is no next section.

# Introduction #

The following section presents general information about MicroCreator. It contains the link to the coding conventions of the project, the directory structure, and the most used data structures.

# The directory structure #

The current directory structure of the project is :

```
.
├── Core
│   ├── Include
│   └── Src
├── example
│   └── empty
├── Libraries
│   ├── allocator
│   │   ├── dedicated_arrays
│   │   └── glibc_malloc
│   ├── energy
│   │   └── esrv
│   ├── power
│   ├── power_snb_msr
│   │   └── includes
│   ├── threadpinner
│   └── timer</pre>
├── obj
├── output
└── regression
```

For the source code of the project, it is found [doxygen here](http://couperin/microbenchmark/microcreator/doc/html/dirs.html).

  * Core: contains the header and source files of the main files
  * example: contains any example files we have with a C file used during the tool's execution
  * obj: is used during the compilation phase to store the object files
  * output: is the default output directory when executing benchmark programs
  * regression: defines any regression tests the tool must pass before modifying the golden version

# The main elements #

## The Main file ##

The Main file contains the _main_ function, handles the command-line options and general tool start-up.

## Core ##

The Core directory contains the internal MicroLauncher code. It consists in files handling the benchmark execution, the configuration file loading and saving, the log system, and the command-line option parsing.

## example ##

The example directory contains a few test examples of kernels MicroLauncher can execute.

## Libraries ##

MicroLauncher is a tool using dynamic libraries to handle two different key components: the allocation and evaluation libraries. An example of allocation library is provided in the allocator subdirectory. The timer subdirectory contains the default evaluation library, using the rtdsc instruction.

# Index Navigation #

This section is part of [Chapter 7: MicroLauncher's Internals](MicroLauncher_Chapter_7_MicroLauncher_Internals.md) of [MicroLauncher's Manual](MicroLauncher.md).

There is no previous section.

There is no next section.