### Index Navigation ###

This section is part of [Chapter 7](MicroCreator_Chapter_7_Internals.md) in the [MicroCreator Manual](MicroCreator.md).

The previous section is [Section 2: Pass Algorithms](MicroCreator_Chapter_7_Internals_Section_2_Pass_Algorithms.md).

The next section is [Section 4: Code Generation](MicroCreator_Chapter_7_Internals_Section_4_Code_Generation.md).

# Introduction #

Plugins are a convenient way to provide expert users techniques to modify the default behavior of any tool and perform their own versions. When a tool exposes its internal functionalities, it can use scripting or plugin systems. Scripting has the advantages of not requiring compilation. The major drawback is scripting requires a different programming language and requires a major effort to bridge both languages. To simplify the users' interactions, MicroCreator uses a plugin system.

A plugin is created in the same language and either presented to the tool as source code or as a dynamic library. In the case of source code, MicroCreator generates a temporary dynamic library. In the case of a dynamic library, the tool loads the plugin at the beginning of the execution into memory and calls an initialization function called pluginInit.

# Plugin Passes #

A plugin pass may add a new pass or modify an existing pass. This flexibility is given via a data structure called SPluginPassInfo. If the plugin adds or modifies a pass, it creates the new pass and initializes the SPluginPassInfo data structure.

In the case of a new pass, the pass is simply added to the list of passes in the PassEngine. After the insertion, the plugin pass is handled as a normal internal pass by MicroCreator.

## Removing a Pass ##

The plugin system also enables users to remove passes. In such cases, the plugin calls directly the driver's function removePass.

# Gate Plugins #

A gate plugin allows the user to modify any gate call. A gate call is performed before any pass is executed. This allows a pass to determine whether to perform a given task on a kernel. However, a user may wish to turn off certain passes and can use the gate plugin system to do so.

A gate plugin is a single gate function that superimposes itself to the general pass system and decides whether or not to execute a pass. At launch time, the gate function is loaded into memory and called before executing any pass:

```
        bool doPass = true;
        if (gateInfo.gate != NULL)
        {
            doPass = gateInfo.gate (description, current);
        }
        else
        {
            doPass = current->gate (description, current->getKernel ());
        }
```

A gate plugin is created by calling the setPluginGate function from the Driver class. This function takes a single SPluginGateInfo data structure.

# Future Plugins #

There is currently a single future work regarding plugins. This regards allowing a user to redefine the input nodes in the XML file. If possible, this would allow a user to fully redefine the behavior of the tool's execution.

# Index Navigation #

This section is part of [Chapter 7](MicroCreator_Chapter_7_Internals.md) in the [MicroCreator Manual](MicroCreator.md).

The previous section is [Section 2: Pass Algorithms](MicroCreator_Chapter_7_Internals_Section_2_Pass_Algorithms.md).

The next section is [Section 4: Code Generation](MicroCreator_Chapter_7_Internals_Section_4_Code_Generation.md).