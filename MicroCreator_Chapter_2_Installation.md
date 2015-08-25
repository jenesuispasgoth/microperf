### Index Navigation ###

This chapter is part of the [MicroCreator Manual](MicroCreator.md).

The previous chapter is [Chapter 1: General Information](MicroCreator_Chapter_1_General_Information.md).

The next chapter is [Chapter 3: General Usage](MicroCreator_Chapter_3_General_Usage.md).

# Introduction #

This chapter presents how to download, install, and first use MicroCreator. The chapter is a quick-reference guide on how to get the tool working on the user's computer.

# Download #

The download and installation of the program is rather simple:

  * Download via GIT: the source can be downloaded via git
    * The Google Code repository contains the current development state of the tool:

> git clone https://code.google.com/p/microperf/

**Another solution is a downloadable version of the ''golden version'':**

> XXX

**Note:** it is recommended to use the golden version since it is considered stable. The rest of this page supposes the use of the stable version.

# Installation #

## Dependencies ##

Currently, MicroCreator requires the following tools or libraries:

### [MicroDetector](MicroDetector.md) ###

  * Most examples suppose microdetect is installed at the same directory level as microcreator

  * MicroDetector is cloned or downloaded with the other tools
    * The installation is straight forward:
```
  $:~/tmp/MicroPerf$ cd microdetect
  $:~/tmp/MicroPerf/microdetect$ make
  g++ -c ArgsInfo.cpp -O3 -Wall -Wextra -g
  g++ -c Driver.cpp -O3 -Wall -Wextra -g
  g++ -c Extractor.cpp -O3 -Wall -Wextra -g
  g++ -c Logging.cpp -O3 -Wall -Wextra -g
  g++ -c Main.cpp -O3 -Wall -Wextra -g
  g++ -o microdetect ArgsInfo.o Driver.o Extractor.o Logging.o Main.o -O3 -Wall -Wextra -g 
```

### [Libxml++-2.6](http://libxmlplusplus.sourceforge.net/) ###
> To install it on an Ubuntu machine:
```
  sudo apt-get install libxml++2.6-dev
```
## Installation ##

First, go in MicroCreator's directory:
```
 $:~/tmp$ cd microcreator/
 $:~/tmp/microcreator$ ls
 Core   examples   Log.txt   Main.h   microcreator   output   parseXML_output.out   regression Doxyfile   Log_detect.txt   Main.cpp   makefile  obj   papers   Passes   utils
```
Second, use make:
```
 $:~/tmp/microcreator$ make

 g++ -c Main.cpp -o obj/Main.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include  `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Core/Src/ArithmeticMDL.cpp -o obj/ArithmeticMDL.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Core/Src/Comment.cpp -o obj/Comment.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Core/Src/Description.cpp -o obj/Description.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Core/Src/DescriptionXML.cpp -o obj/DescriptionXML.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include `pkg-config libxml++-2.6 --cflags  --libs`
 g++ -c Core/Src/Driver.cpp -o obj/Driver.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Core/Src/ExecutionOptions.cpp -o obj/ExecutionOptions.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Core/Src/HWInformation.cpp -o obj/HWInformation.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Core/Src/ImmediateOperand.cpp -o obj/ImmediateOperand.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Core/Src/IndirectMemoryOperand.cpp -o obj/IndirectMemoryOperand.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Core/Src/InductionOperand.cpp -o obj/InductionOperand.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Core/Src/InsertCode.cpp -o obj/InsertCode.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Core/Src/Instruction.cpp -o obj/Instruction.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Core/Src/InstructionMDL.cpp -o obj/InstructionMDL.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Core/Src/Kernel.cpp -o obj/Kernel.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Core/Src/Logging.cpp -o obj/Logging.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Core/Src/MemoryMDL.cpp -o obj/MemoryMDL.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Core/Src/MemoryOperand.cpp -o obj/MemoryOperand.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Core/Src/Operand.cpp -o obj/Operand.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Core/Src/Operation.cpp -o obj/Operation.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Core/Src/ParserXML.cpp -o obj/ParserXML.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Core/Src/Pass.cpp -o obj/Pass.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Core/Src/PassElement.cpp -o obj/PassElement.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Core/Src/PassEngine.cpp -o obj/PassEngine.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Core/Src/PluginPass.cpp -o obj/PluginPass.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Core/Src/RegisterOperand.cpp -o obj/RegisterOperand.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Core/Src/RegularRegisterOperand.cpp -o obj/RegularRegisterOperand.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Core/Src/Statement.cpp -o obj/Statement.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Passes/Src/CodeGeneration.cpp -o obj/CodeGeneration.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include  `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Passes/Src/ImmediateSelection.cpp -o obj/ImmediateSelection.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include  `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Passes/Src/InductionInsertion.cpp -o obj/InductionInsertion.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include  `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Passes/Src/InductionSelection.cpp -o obj/InductionSelection.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include  `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Passes/Src/MDLScheduling.cpp -o obj/MDLScheduling.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include  `pkg-config libxml++-2.6 --cflags  --libs`
 g++ -c Passes/Src/MDLSizeSplit.cpp -o obj/MDLSizeSplit.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include  `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Passes/Src/MinMaxBundle.cpp -o obj/MinMaxBundle.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include  `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Passes/Src/OMPCode.cpp -o obj/OMPCode.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include  `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Passes/Src/OperandSwap.cpp -o obj/OperandSwap.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include  `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Passes/Src/OperationChoose.cpp -o obj/OperationChoose.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include  `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Passes/Src/PassMDL.cpp -o obj/PassMDL.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include  `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Passes/Src/PassUnroll.cpp -o obj/PassUnroll.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include  `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Passes/Src/RegisterAllocation.cpp -o obj/RegisterAllocation.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include  `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Passes/Src/RegisterAllocationMDL.cpp -o obj/RegisterAllocationMDL.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include  `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Passes/Src/StatementSelection.cpp -o obj/StatementSelection.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include  `pkg-config libxml++-2.6 --cflags --libs`
 g++ -c Passes/Src/StrideSelection.cpp -o obj/StrideSelection.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include  `pkg-config libxml++-2.6 --cflags --libs`
 g++ -o microcreator obj/Main.o obj/ArithmeticMDL.o obj/Comment.o obj/Description.o obj/DescriptionXML.o obj/Driver.o obj/ExecutionOptions.o obj/HWInformation.o 
 obj/ImmediateOperand.o obj/IndirectMemoryOperand.o obj/InductionOperand.o obj/InsertCode.o obj/Instruction.o obj/InstructionMDL.o obj/Kernel.o obj/Logging.o
 obj/MemoryMDL.o obj/MemoryOperand.o obj/Operand.o obj/Operation.o obj/ParserXML.o obj/Pass.o obj/PassElement.o obj/PassEngine.o obj/PluginPass.o obj/RegisterOperand.o
 obj/RegularRegisterOperand.o obj/Statement.o obj/CodeGeneration.o obj/ImmediateSelection.o obj/InductionInsertion.o obj/InductionSelection.o obj/MDLScheduling.o
 obj/MDLSizeSplit.o obj/MinMaxBundle.o obj/OMPCode.o obj/OperandSwap.o obj/OperationChoose.o obj/PassMDL.o obj/PassUnroll.o obj/RegisterAllocation.o
 obj/RegisterAllocationMDL.o obj/StatementSelection.o obj/StrideSelection.o -O3 -Wall -Wextra -g -ICore/Include -IPasses/Include `pkg-config libxml++-2.6 --cflags --libs` -Wl,-E
```
In order to check whether the executable was created:
```
 $:~/tmp/microcreator$ ls
 Core  Doxyfile  examples  Log_detect.txt  Main.cpp  Main.h  makefile  microcreator  obj  output  papers  Passes  Plugins  regression

= Displaying the Help =

Once MicroCreator is built, it is possible to run the program. With the --help command-line options, the software prints out the command-line option possibilities:

 $:./microcreator --help
 NAME
 	MicroCreator - micro-benchmarks generator 
 
 SYNOPSIS
 	./microcreator   [DESCRIPTION_PATH] [--version] [--help] [--verbose] 
 
 			 [--brief] [--asm] [--list-passes] [--fplugin=PLUGIN_PATH] 
 
 DESCRIPTION
 	MicroCreator is a benchmark programs creator. It is used to create programs with slight differences in order to analyze these slight changes' impact on the underlying architecture. It is a C++ project and a stand-alone program.
 
 OPTIONS
 	--version, -v
 		Prints the MicroCreator version.
 
 	--help, -h
 		Prints the synopsis and a list of the different options.
 
 	--verbose, -v
 		Sets the system into a verbosis tool. It creates an output file for each pass, giving extra information on what has happenned.
 
 	--brief, -b
 		Sets the system into a silent mode. It disables the verbosis made.
 
 	--asm, -c
 		Generates C code using asm instructions in the kernel.
 
 	--list-passes, -l
 		Displays the list of passes currently used.
 
 	--fplugin=<path/to/plugin.so>
 		Call a plugin.
 
 	--n <number> 
 		Set the start file number counter.
 
 DOCUMENTATION
 	The documentation for the MicroCreator tool is available at http://code.google.com/p/microperf.
```
# First Usage #

The software comes with an ''examples'' directory. When using one of the files in the directory, the tool outputs:
```
 $:~/tmp/microcreator$ ./microcreator examples/description_MOVAPD_st_L_LL_LLL.xml 
 Running 100 % 
 Log last lines:
 Stopping Register Allocation
 Got a new kernel output 0x1097480:
 Starting new kernel pass
 Now execute the passCode Generation
 Starting Code Generation
 Opening file: output/example0509.s
 Stopping Code Generation
 PassEngine is stopping
 Drove
 Micro-creator shutting down
```

The tool outputs the last lines of the log file which shows MicroCreator generated 510 new benchmarks and they were created in the output directory.

# Index Navigation #

This chapter is part of the [MicroCreator Manual](MicroCreator.md).

The previous chapter is [Information Chapter 1: General Information](MicroCreator_Chapter_1_General_Information.md).

The next chapter is [Chapter 3: General Usage](MicroCreator_Chapter_3_General_Usage.md)].