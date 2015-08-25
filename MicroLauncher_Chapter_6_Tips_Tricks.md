### Index Navigation ###

This chapter is part of the [MicroLauncher Manual](MicroLauncher.md).

The previous chapter is [Chapter 5: Error Messages](MicroLauncher_Chapter_5_Error_Messages.md).

The next chapter is [Chapter 7: MicroLauncher's Internals](MicroLauncher_Chapter_7_Internals.md).

# Introduction #

Through the use of Microlauncher, tips and tricks on how to get stable results emerge from user experience and gained knowledge. Consider the chapter as a feedback section of the tool's manual since it builds itself from exchanges between developers and users.

# Stability #

Even though MicroLauncher is defined as a micro-benchmark launcher, which executes programs in a closed and controlled environment, stability in results is not a trivial task. The use of execution, meta, and base repetitions is one of the solutions in obtaining stable and reproducible results. In the case of micro-benchmarks, users determined:

  * The execution repetition provides reproducibility and does not help stabilize results

  * The meta-repetition helps determine minimum, median, and maximum values
    * The variance between these values shows the stability itself
    * Users often prefer thirty meta-repetitions because it is the threshold to the normal law's distribution

  * The base repetition is the center of stability in the micro-benchmark programs
    * The value depends on the array size and users fine tune it to the following:
      * **3000 for 16kB to 512kB of array's elements
      *** 50 for large arrays, ie more than 24 million bytes of elements

The above values are true for current Xeon architectures with a 32kB first level cache, 256kB second level cache, and 12MB third level cache.

# Array Sizes #

The array sizes chosen by users generally depend on the target architecture. MicroLauncher often simulates the access cost of an array residing in one of the memory hierarchy levels. In order to achieve it, the size chosen is half the memory hierarchy size. For example, with a 32kB first level cache, the array size should be 16kB. Depending on the data type, it may represent sixteen thousand characters, four thousand single-precision floating point numbers, etc.

Instability occurs when the array size is close to the size of the lower memory hierarchy or the targeted memory hierarchy. In the case of a 32kB first level cache and 128kB second level cache, instability, when targeting the second level, happens when the array size is less than 40kB and higher than 90kB.

Kernels executing linear access patterns provided the previous numbers. In more complex kernels, it is possible the numbers require tweaking.

# Iteration Count #

MicroLauncher's kernel iteration count helps calculate the cost per iteration. It is important, if using the per iteration feature, to return the correct number of iterations. Though MicroLauncher passes to the kernel the number of elements and the allocated arrays, it is still the kernel's job to actually return the real iteration count since striding can occur.

The following code listing shows the general code a user should write. Since the loop increments the induction variable by 2 per iteration, the actual iteration count is half of arraySize. Either the user can return the value if the kernel's code is stable or add the explicit iteration counter.

```
 int iterCount = 0;
 
 for (i = 0; i < arraySize; i += 2)
 {
   ...
   iterCount++;
 }
 
 return iterCount;
```

# Multi-process #

Another feature interesting users is multi-process execution. In order to stress the memory components of a multi-core architecture, it is sometimes necessary to have each core access the memory at the same time. Users generally either execute the kernel on one core or on every core. However, the tool, combined with an energy metric, also can determine how many cores are necessary to transform a code from a CPU bound code to a memory bound code.

# Conclusion #

In conclusion, the current chapter is a summary of comments and ways Microlauncher is used. It is a an evolving chapter as users start trying new techniques, methodologies, and tests. As conversations and experiments get relayed to the developers, the chapter will evolve.

# Index Navigation #

This chapter is part of the [MicroLauncher Manual](MicroLauncher.md).

The previous chapter is [Chapter 5: Error Messages](MicroLauncher_Chapter_5_Error_Messages.md).

The next chapter is [Chapter 7: MicroLauncher's Internals](MicroLauncher_Chapter_7_Internals.md).