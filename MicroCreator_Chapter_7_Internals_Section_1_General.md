### Index Navigation ###

This section is part of [Chapter 7](MicroCreator_Chapter_7_Internals.md) in the [MicroCreator Manual](MicroCreator.md).

There is no previous section.

The next section is [Section 2: Pass Algorithms](MicroCreator_Chapter_7_Internals_Section_2_Pass_Algorithms.md).

# Introduction #

This section presents general information about MicroCreator. It contains the link to the coding conventions of the project, the directory structure, and the most used data structures.

# The directory structure #

The current directory structure of the project is :

```
├── Core
│   ├── Include
│   └── Src
├── examples
│   ├── OpenMP
│   └── PluginExamples
│       ├── Gate
│       └── Pass
├── obj
├── output
├── papers
│   ├── cgo12
│   └── sc11
├── Passes
│   ├── Include
│   └── Src
└── regression
```

  * Core: contains the header and source files of the main files

  * examples: contains any example files we have with two sub-directories: OpenMP and PluginExamples for specific examples

  * obj: is used during the compilation phase to store the object files

  * output: is the default output directory when generating benchmark programs

  * papers: is used to store papers relating to the project

  * Passes: contains all the specific default passes that microcreator has

  * regression: is used to define any regression tests that the tool must pass before modifying the golden version

# The main elements #

## The Main file ##

The Main file contains the _main_ function and parses the XML input file, loads the passes and any plugins then starts the driver.

## The Driver ##

The Driver is used as a container of the various default passes. A pass is an algorithm that is applied to a given kernel. The driver contains a pass engine that contains the list of passes to apply to a given kernel. Each pass takes a kernel as an entry and can output multiple kernels. This 1-entry, multiple outputs can lead to an exponential number of generated programs. It is used to load up the default passes via the _addGeneralPasses_ function and to start the generation process via its _drive_ function. This function simply calls the passEngine's _drivePasses_ function.

## The PassEngine ##

The PassEngine contains a vector of Passes that are called one after another for a given Kernel. The PassEngine is responsible for calling a pass, retrieving the newly generated kernels from that pass and adding them to a list of kernels.

The main function for the PassEngine class is the drivePasses function.

## A PassElement ##

A PassElement is used to contain information such as: the associated kernel, what is the next pass to execute. The PassElement's main function is the entry function.

This function's job is to extract the current pass, call the pass for the given kernel and then return the vector list of new PassElements, each containing the next Pass to execute and the newly generated kernels.

## A Kernel ##

A Kernel is a Statement. It defines a group of Statements and Induction variables.

## A Statement ##

A Statement is the generic element that a Kernel contains. It is defined as the generic element that can be specialized as this image shows:

# Index Navigation #

This section is part of [Chapter 7](MicroCreator_Chapter_7_Internals.md) in the [MicroCreator Manual](MicroCreator.md).

There is no previous section.

The next section is [Section 2: Pass Algorithms](MicroCreator_Chapter_7_Internals_Section_2_Pass_Algorithms.md).