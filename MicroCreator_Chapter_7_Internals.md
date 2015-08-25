### Index Navigation ###

This chapter is part of the [MicroCreator Manual](MicroCreator.md).

The previous chapter is [Chapter 6: The Plugin System](MicroCreator_Chapter_6_The_Plugin_System.md).

There is no next chapter.

# Introduction #

The current chapter, as opposed to [Chapter 3](MicroCreator_Chapter_3_General_Usage.md) contains information such as the internals of the tool, how the source code is written, what algorithms were used to complete certain tasks, etc. It is defined as the Developer's Chapter since it is useful for anyone who wants additional information on how things were programmed or hidden from the general user. The chapter should be read by anyone who wants to modify the code or understand it.

# Chapter 7 Index #

This chapter contains a lot of information. To facilitate its reading, it is divided into sections.

## [Section 1: General](MicroCreator_Chapter_7_Internals_Section_1_General.md) ##

[Section 1: General](MicroCreator_Chapter_7_Internals_Section_1_General.md) presents coding conventions and general data structures involved in the tool. It is important to clearly understand these main structures since they are at the heart of any modifications to MicroCreator's code base.

## [Section 2: Pass Algorithms](MicroCreator_Chapter_7_Internals_Section_2_Pass_Algorithms.md) ##

MicroCreator is a source-to-source compiler and contains many passes to modify the input file into source code. [Section 2: Pass Algorithms](MicroCreator_Chapter_7_Internals_Section_2_Pass_Algorithms.md) explains each pass in detail. A pass takes as input a kernel and modifies it and even creates new kernels. Though the codes of the passes are not explained, the basic algorithms and the purpose of each pass is.

## [Section 3: Plugin System](MicroCreator_Chapter_7_Internals_Section_3_Plugin_System.md) ##

[Section 3: Plugin System](MicroCreator_Chapter_7_Internals_Section_3_Plugin_System.md) explains the plugin system, which allows a user to modify the behavior of MicroCreator without having to change a single line of the core program. The section presents how the plugin system is integrated into the tool and what this represents for the core developer.

## [Section 4: Code Generation](MicroCreator_Chapter_7_Internals_Section_4_Code_Generation.md) ##

[Section 4: Code Generation](MicroCreator_Chapter_7_Internals_Section_4_Code_Generation.md) describes the different aspects revolving around code generation. Differences between how MicroCreator generates assembly or C programs are presented. The OpenMP code generation is also discussed.

# Index Navigation #

This chapter is part of the [MicroCreator Manual](MicroCreator.md).

The previous chapter is [Chapter 6: The Plugin System](MicroCreator_Chapter_6_The_Plugin_System.md).

There is no next chapter.