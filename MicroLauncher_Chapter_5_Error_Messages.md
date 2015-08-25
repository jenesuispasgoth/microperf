### Index Navigation ###

This chapter is part of the [MicroLauncher Manual](MicroLauncher.md).

The previous chapter is [Chapter 4: User Options](MicroLauncher_Chapter_4_User_Options.md).

The next chapter is [Chapter 6: Tips and Tricks](MicroLauncher_Chapter_6_Tips_Tricks.md).

# Introduction #

MicroLauncher, as any tool, provides error messages the user must decrypt. The current chapter is a presentation of error messages and how to fix the issues.

# Kernel #

Kernel error messages occur when MicroLauncher is in kernel mode. Currently, there are two major error messages: when the function name provided does not exist and when the initialization function does not exist.

## Kernel Function ##

```
 Error: kernel function X does not exist
```

The function provided by kernelfunction does not exist in the kernel. MicroLauncher should print out the existing functions.

## Kernel Init Function ##

```
 Error: kernel init function X does not exist
```

The kernel initialization function provided by the initfunction option does not exist in the kernel.

# Libraries #

MicroLauncher integrates user-defined libraries. When loading the libraries or calling the functions, MicroLauncher can encounter errors.

# Library Loading #

```
 Error: Library name ...
```

An error when opening one of the dynamic libraries occurred. The message should finish by providing the error given by the dynamic library handler.

## Evaluation Library ##

```
 Warning: X function is not declared in Evaluation Library
```

MicroLauncher uses the evaluation library to calculate metrics. It requires four functions to perform its task, each necessary for MicroLauncher:

  * evaluationStart

  * evaluationStop

  * evaluationInit

  * evaluationClose

## Verification Library ##

```
 Error: Cannot load X in the verification library Y : Z
```

As for the evaluation library, the verification library requires a certain number of functions:

  * verificationInit

  * verificationDisplay

  * verificationDestroy

## Allocation Library ##

```
 Error while loading dynamic allocation library name : not all functions could be found 
```

As for the previous libraries, the allocation library requires a certain number of functions:

  * allocationInit

  * allocationMalloc

  * allocationFree

  * allocationClose

# Unexplained Errors #

If ever, during the use of the tool, the reader finds an obscure unexplained error message. Please let us know at microtools at exascale dash computing dot eu. The tool authors will answer the email as soon as possible and update the error message page.

# Index Navigation #

This chapter is part of the [MicroLauncher Manual](MicroLauncher.md).

The previous chapter is [Chapter 4: User Options](MicroLauncher_Chapter_4_User_Options.md).

The next chapter is [Chapter 6: Tips and Tricks](MicroLauncher_Chapter_6_Tips_Tricks.md).