### Index Navigation ###

This chapter is part of the [MicroLauncher Manual](MicroLauncher.md).

There is no previous chapter.

The next chapter is [Chapter 2: Installation and First Use](MicroLauncher_Chapter_2_Installation.md).

# Introduction #

Timing programs, whether microbenchmark programs created by [Tools:MicroCreator MicroCreator](Microbenchmark.md) or stand-alone applications, is generally a complex process.  There are a lot of elements to handle and, when the user wants to time multicore executions, the risk of a mistake in the process augments.  Furthermore, timing for performance is not the only information users want to know; peak power usage or energy consumption are other interesting factors.  Considering Exascale machines contain millions of cores the interest in energy related metrics heightens.

# Purpose #

MicroLauncher provides a generic framework to use as many metrics as possible around the execution of a benchmark program or a stand-alone executable.  MicroLauncher integrates one performance metric using the rdtsc register.  However, MicroLauncher allows the user to define a new metric easily by defining a few functions.

Stand-alone execution, by definition, means MicroLauncher executes a program and measures a given metric.  MicroLauncher also permits the execution of the program on multiple cores.  Such an execution simulates the effect of an identical program, such as an MPI task, running on different cores.  The tool also repeats the experiment a number of times to ensure the stability of the results.

An entry point function in the program define the start of a microbenchmark program.  MicroLauncher automatically compiles the program and calls the entry point function.  The tool also allocates arrays and handles their alignment depending on the needs of the benchmark program.  MicroLauncher allocates up to eight arrays and, because microbenchmarks are generally short in terms of execution time, a repetition factor is used to augment the execution time.  As in the stand-alone case, the tool also repeats the experiment a number of times to check for stability.

# Manual's Structure #

Seven chapters divide the user manual for MicroLauncher, ordered by user experience.  A first user of the tool should read Chapters 1 through 5 because the chapters explain the tool's general usage, the options, and the general error messages.  An intermediate user should read Chapter 6 because it presents tips and tricks to stabilize the results and ensure reliable measures.  Finally, experienced users might want to modify the general behavior of the tool; therefore need to read Chapter 7 to do so.  The last chapter describes how the tool implements metric and allocation systems, how to modify the algorithms, the general layout of the code, etc.

# Conclusion #

MicroLauncher's a tool with approximately ten thousand lines of code.  However, with more than forty options, the tool's is very versatile and requires a full explanation.  The manual provides explanations for each option, how they interact with each other, and how to use the tool correctly.

# Index Navigation #

This chapter is part of the [MicroLauncher Manual](MicroLauncher.md).

There is no previous chapter.

The next chapter is [Chapter 2: Installation and First Use](MicroLauncher_Chapter_2_Installation.md).