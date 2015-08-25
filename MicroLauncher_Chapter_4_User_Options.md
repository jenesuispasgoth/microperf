### Index Navigation ###

This chapter is part of the [MicroLauncher Manual](MicroLauncher.md).

The previous chapter is [Chapter 3: General Usage](MicroLauncher_Chapter_3_General_Usage.md).

The next chapter is [Chapter 5: Error Messages](MicroLauncher_Chapter_5_Error_Messages.md).

# Options #

There are currently many options to modify the default behavior of MicroLauncher.  The current chapter presents each option and, when required, shows via an example what the option does.  There are three option groups: kernel mode, stand-alone mode, and common options to both modes.

## [Section 1: Kernel Mode](MicroLauncher_Chapter_4_User_Options_Section_1_Kernel_Mode.md) ##

[Section 1: Kernel Mode](MicroLauncher_Chapter_4_User_Options_Section_1_Kernel_Mode.md) shows the kernel mode options.  Users choose the kernel mode when the microbenchmark program is in assembly form and need a launching framework to determine performance or energy consumption.  In the kernel mode, MicroLauncher allocates the arrays, initializes them, and modifies the total number of iterations.

## [Section 2: Stand-Alone Mode](MicroLauncher_Chapter_4_User_Options_Section_2_Stand_Alone_Mode.md) ##

[Section 2: Stand-Alone Mode](MicroLauncher_Chapter_4_User_Options_Section_2_Stand_Alone_Mode.md) is for users who have full programs they wish to test.  MicroLauncher then serves as a launching framework for the program but does not handle data allocation or modify the number of iterations.  The tool launches and measures the program with the chosen metric.

## [Section 3: Common Options](MicroLauncher_Chapter_4_User_Options_Section_3_Common_Options.md) ##

[Section 3: Common Options](MicroLauncher_Chapter_4_User_Options_Section_3_Common_Options.md) explains the different options common to both modes.  For example, common to both modes are the choice of the metric used or the amount of information MicroLauncher outputs.

# Conclusion #

Though MicroLauncher contains currently more than forty-five options, its use is relatively simple because they are all optional.  A beginner user starts using MicroLauncher without any of the options then as the tool becomes more familiar, the user may start wanting to tweak the use and behavior of the tool.

# Index Navigation #

This chapter is part of the [MicroLauncher Manual](MicroLauncher.md).

The previous chapter is [Chapter 3: General Usage](MicroLauncher_Chapter_3_General_Usage.md).

The next chapter is [Chapter 5: Error Messages](MicroLauncher_Chapter_5_Error_Messages.md).