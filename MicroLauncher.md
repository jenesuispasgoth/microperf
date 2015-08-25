# Introduction #

MicroLauncher launches synthetic benchmark programs.  It executes programs with slight differences and help analyze the slight changes' impact on the underlying architecture.  The goal being to aid hardware and software architects better comprehend the execution of software on the hardware.  It is a C project and a stand-alone program.

## Context ##

MicroLauncher is one of the [MicroPerf Tools](MicroPerf.md) divided into three sub-projects:

**[MicroCreator](MicroCreator.md) generates benchmark programs relating to memory**

**MicroLauncher\_executes benchmarks, obtaining consistent results**

**[MicroDetector](MicroDetector.md) detects architectural elements of the machine**

# User Manual Index #

## [Chapter 1: General Information](MicroLauncher_Chapter_1_General_Information.md) ##

The creation of MicroLauncher originated with the work on a tool called Kerbe, which allowed users to try different data allocation methods and test different microbenchmark programs.  However, the user would recompile each test program, making it unusable for a beginner.  MicroLauncher replaced the original tool by allowing users to directly integrate their benchmark program into Kerbe and retrieve performance evaluation.  [Chapter: 1](MicroLauncher_Chapter_1_General_Information.md) enumerates the pros and cons of MicroLauncher.

## [Chapter 2: Installation and First Use](MicroLauncher_Chapter_2_Installation.md) ##

[Chapter 2](MicroLauncher_Chapter_2_Installation.md) explains where to download or retrieve MicroLauncher's source code and how to install it.  It also gives links to external dependencies, which are required for correct execution of the tool.  Finally, through examples provided with the tool, the chapter illustrates the first few uses of MicroLauncher.

## [Chapter 3: General Usage](MicroLauncher_Chapter_3_General_Usage.md) ##

[Chapter 3](MicroLauncher_Chapter_3_General_Usage.md) goes through the main usage of the tool.  Once the user installs the tool, users generally want to use the most common options and start analyzing their programs.  The General Usage chapter is a presentation of the tool for these new users.  It presents the major options and what to expect as output.  It also shows how to post-process the results in order to obtain performance graphs.

## [Chapter 4: User Options](MicroLauncher_Chapter_4_User_Options.md) ##

[Chapter 4](MicroLauncher_Chapter_4_User_Options.md) breaks down every user option of MicroLauncher.  There are currently more than thirty to tune the behavior of the tool.  The User Options chapter defines each option, what it modifies, and its default value.  MicroLauncher supports two types of input programs\_assembly kernels and full executable applications. Therefore, there are specific options to each case. Finally, the chapter presents the options available in both modes.

## [Chapter 5: Error Messages](MicroLauncher_Chapter_5_Error_Messages.md) ##

[Chapter 5](MicroLauncher_Chapter_5_Error_Messages.md) presents each error message MicroLauncher provides to users. It also gives insight as to what caused each error and how to fix it.

## [Chapter 6: Tips and Tricks](MicroLauncher_Chapter_6_Tips_Tricks.md) ##

[Chapter 6](MicroLauncher_Chapter_6_Tips_Tricks.md) is the expert user chapter because it shows tips and tricks concerning MicroLauncher's use and lessons current users have already learned.

## [Chapter 7: MicroLauncher's Internals](MicroLauncher_Chapter_7_Internals.md) ##

[Chapter 7](MicroLauncher_Chapter_7_Internals.md) presents MicroLauncher's code structure for future contributors or people interested in how elements were programmed in the tool.