### Index Navigation ###

This section is part of [Chapter 4](MicroLauncher_Chapter_4_User_Options.md) of [MicroLauncher's Manual](MicroLauncher.md).

The previous section is [Section 1: Kernel Mode](MicroLauncher_Chapter_4_User_Options_Section_1_Kernel_Mode.md).

The next section is [Section 3: Common Options](MicroLauncher_Chapter_4_User_Options_Section_3_Common_Options.md).

# Introduction #

The current page presents user options for MicroLauncher regarding the stand-alone mode. Stand-alone mode is for full applications requiring a launcher for multi-core execution.

# Executable #

## Application's File ##

The execname option defines the application's executable file. The file must be executable from an operating system standpoint or a compiler must successfully generate an executable file.

Example:

**Command-Line**
```
 ./microlaunch --execname superapp
 ./microlaunch --execname example/stand-alone-sleep.c
```

**Configuration File**
```
 <execName value="superapp" />
```

## Executable Arguments ##

An application sometimes requires command-line arguments. The option **execargs** defines the arguments for the application and are passed to it when launched.

**Command-Line**
```
 ./microlaunch --execname superapp --execargs="-o output -i input.data"
```

**Configuration File**
```
 <execArgs value="-o output -i input.data" />
```

## Executable Output ##

Certain applications output information to the user. To redirect the output into a file, the user may use the execoutput option.

**Command-Line**
```
 ./microlaunch --execname superapp --execoutput execOutput.log
```

**Configuration File**
```
 <execOutput value ="execOutput.log" />
```

## Suppressing the Output ##

On the other hand, sometimes it is better to suppress the output of an application.  To suppress an application's output, add the option **suppress-output**.

**Command-Line**
```
 ./microlaunch --execname superapp --suppress-output
```

**Configuration File**
```
 <suppressOutput/>
```

# Conclusion #

Since a stand-alone application is independent from MicroLauncher, there are less options for tweaking execution behavior. However, the options available do provide the possibility to pin, run multiple instances, and modify the output behavior. The next section, Common Options, presents the common options between both modes.

# Index Navigation #

This section is part of [Chapter 4](MicroLauncher_Chapter_4_User_Options.md) of [MicroLauncher's Manual](MicroLauncher.md).

The previous section is [Section 1: Kernel Mode](MicroLauncher_Chapter_4_User_Options_Section_1_Kernel_Mode.md).

The next section is [Section 3: Common Options](MicroLauncher_Chapter_4_User_Options_Section_3_Common_Options.md).