# Introduction #

This tool creates synthetic benchmark programs. It is used to create programs with slight differences in order to, with [MicroLauncher](MicroLauncher.md), analyze these slight changes' impact on the underlying architecture. It is a C++ project and a stand-alone program.

## Context ##

MicroCreator is one of the [microbenchmark tools](MicroPerf.md). The tool project is divided into three sub-projects:

  * [MicroCreator](MicroCreator.md): generating benchmark programs relating to memory
  * [MicroLauncher](MicroLauncher.md): executing benchmarks and obtaining consistent results
  * [MicroDetector](MicroDetector.md): detecting architectural elements of the machine

# User Manual Index #

## [Chapter 1: General Information](MicroCreator_Chapter_1_General_Information.md) ##

The reasons behind MicroCreator's, a tool for benchmark generation, development is presented in [Chapter 1: General Information](MicroCreator_Chapter_1_General_Information.md). It also explains how the tool helps users understand performance issues and how to analyze them using the other Microbenchmark tools.

## [Chapter 2: Installation and First Use](MicroCreator_Chapter_2_Installation.md) ##

[Chapter 2: Installation and First Use](MicroCreator_Chapter_2_Installation.md) explains how to install the tool. This chapter illustrates the tool's usage with examples explaining different elements of the input and how the inputs can modify the tool's behavior. The tool's XML input and its specifications are presented in [Chapter 4](MicroCreator_Chapter_4_Input_Specification.md). Certain external library dependencies are required for installation. These library dependencies and where they can be found are included in this chapter. Finally, the tool is available via download or the GIT repositories.

## [Chapter 3: General Usage](MicroCreator_Chapter_3_General_Usage.md) ##

[Chapter 3: General Usage](MicroCreator_Chapter_3_General_Usage.md) presents in greater detail the usage of MicroCreator. Once the user understands how to install and use the tool at least once, they generally want to modify the example files to change the microbenchmark generation. In order to do so, the intermediate user must understand the input file's format and the various command-line options.

## [Chapter 4: Input Specification](MicroCreator_Chapter_4_Input_Specification.md) ##

[Chapter 4](MicroCreator_Chapter_4_Input_Specification.md) explains each input file option. Since the input file is in XML format, this chapter presents each possible XML node and what they represent for the tool. Authorized values are also explained. Examples are given when necessary to illustrate how each XML node is used and the impacts.

## [Chapter 5: Error Messages](MicroCreator_Chapter_5_Error_Messages.md) ##

[Chapter 5](MicroCreator_Chapter_5_Error_Messages.md) lists possible error messages provided by the tool. One major source of error messages are input file mistakes. This chapter presents the messages that may appear, their meaning, and how to fix them.

## [Chapter 6: The Plugin System](MicroCreator_Chapter_6_The_Plugin_System.md) ##

[Chapter 6](MicroCreator_Chapter_6_The_Plugin_System.md) shows how to use MicroCreator's plugin system. The plugin system allows an advanced user to write their own passes. Since MicroCreator is essentially a source-to-source compiler, the tool is divided into passes for modifying the initial input description into assembly or C code. The advanced user might wish to write a new instruction scheduling algorithm, change which pass is executed, or even create a new XML input node.

## [Chapter 7: MicroCreator's Internals](MicroCreator_Chapter_7_Internals.md) ##

[Chapter 7](MicroCreator_Chapter_7_Internals.md) presents the tools' internals. This chapter is for expert users wishing to work either in the source code of the tool or write advanced plugin systems. It presents the actual passes and their algorithms as well as the general data structures used in the tool. After reading this chapter, the user will have a global understanding of everything under MicroCreator's hood.