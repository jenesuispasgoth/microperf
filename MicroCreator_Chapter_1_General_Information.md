### Index Navigation ###

This chapter is part of the [MicroCreator Manual](MicroCreator.md).

There is no previous chapter.

The next chapter is [Chapter 2: Installation and First Use](MicroCreator_Chapter_2_Installation.md).

# Introduction #

MicroCreator is a benchmark program generator. It creates microbenchmark programs from configuration files given by the user. These benchmarks are executed as stand-alone programs or integrated into a driver. A stand-alone program is useful since it allows the user to deploy the program on various architectures, test it with different compiler options, and study the whole application. However, since it is a stand-alone program, the responsibility of creating a stable environment and get reproducible performance evaluation metrics falls upon the user. Alternatively, if MicroCreator is integrated into a working and proven driver, the user has the advantage of trusting the performance results. The primary purpose of MicroCreator is studying memory issues.

# Memory Issues #

There are many different memory issues addressed by the ''Memory Characterization'' project, for example:

  * How stride accesses affect performance

  * Prefetching issues:
    * Turned on or off for certain memory streams
    * Accuracy
    * How prefetching is handled in the multi-core environment

  * 4k aliasing: is when the processor checks for conflicts between certain load and store instructions
    * on certain processors, there is a check on the 12 least-significant bits of the addresses
    * if the value is the same, the processor determines if there is a conflict among a store and a load

  * Determine cache sizes and associativity

  * Application performance when modifying the hardware
    * Cache size
    * Cache associativity

# Index Navigation #

This chapter is part of the [MicroCreator Manual](MicroCreator.md).

There is no previous chapter.

The next chapter is [Chapter 2: Installation and First Use](MicroCreator_Chapter_2_Installation.md).