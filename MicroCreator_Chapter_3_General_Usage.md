### Index Navigation ###

The following chapter is part of the [MicroCreator Manual](MicroCreator.md).

The previous chapter is [Chapter 2: Installation and First Use](MicroCreator_Chapter_2_Installation.md).

The next chapter is [Chapter 4: Input Specification](MicroCreator_Chapter_4_Input_Specification.md).

# Introduction #

MicroCreator is a tool used to generate synthetic benchmarks. They are called _synthetic_ since they are hand-tailored and not extracted from real-world applications. The benchmarks are generated with slight changes in order to study the impact of the changes on performance, energy consumption, and other factors. The tool uses an XML-based input file and generates either assembly code or C code, depending on options in the XML file or the command-line options.

As presented in [Chapter 1: General Information](MicroCreator_Chapter_1_General_Information.md), the use of the _golden version_ is supposed. The _golden version_ represents a stable state of the tool.

MicroCreator creates, from XML files, assembly or C code, generating variations of a given code structure. The creation process is done using an internal pass system and a user can also define new passes or modify existing ones to further enhance the entire system. The philosophy of the tool is its ease for the end-user. The following chapter presents how the tool's functions and, via examples, what is the general behavior of the tool.

# Input and Output #

The following section presents the input used by the tool and briefly specifies how these input files interact with MicroCreator. Second, it explains the expect output for the generated files.

## Input ##

As input, the tool uses a XML format defined in the: [MicroCreator\_Chapter\_4\_Input\_Specification|Chapter 4: Input Specification]].

An input file contains four different parts of information:

  * Prologue information

  * Epilogue information

  * Code to be generated

  * Hardware detection system

### Prologue and Epilogue ###

The prologue and epilogue are actually defined by the user for the generated code. Since the system generates assembly or C code, it is important to provide the outer elements of the generated code. For example, for a C program, a prologue includes the required header files, the function's signature, potentially some initial work for setting up data. For an assembly code, the prologue includes setting up the registers correctly or setting the stack.

MicroCreator's purpose is not to provide the prologue and epilogue for the user since the two are generally linked tightly to the actual to-be generated code. However, during testing and internal usage, _examples/prologue.s_ and _examples/epilogue.s_ were built for examples.

## Output ##

The output is generated code, whether assembly or C code. The tool does not verify the code correctness, it is the user's responsibility to ensure the content of the prologue and epilogue files are correct and MicroCreator's generated code does what the user intended.

# Examples of Usage #

The current section presents a couple of examples from the example directory.

## Input Files ##

There are a lot of example files in the example directory which are useful for seeing what the tool generates. The rest of the section shows a couple of examples in greater detail. However, more information can be found in [4\_Input\_Specification|Chapter 4: Input Specification](MicroCreator_Chapter.md)].

## Description\_MOVAPD\_St\_L\_LL\_LLL.Xml ##

The first example _description\_MOVAPD\_st\_L\_LL\_LLL.xml_ creates 510 programs because it generates the following regular expression (Store|Load){1-8}.

When running the program, it outputs:

```
 $:~/Memory/microcreator$ ./microcreator examples/description_MOVAPD_st_L_LL_LLL.xml 
 Log last lines:
 Stopping Register Allocation
 Got a new kernel output 0x975c50:
 Starting new kernel pass
 Now execute the passCode Generation
 Starting Code Generation
 Opening file: output/example0509.s
 Stopping Code Generation
 PassEngine is stopping
 Drove
 Micro-creator shutting down
```

### Input File ###

After the initial _description_ node, the first part inserts the prologue file:
```
    <kernel>
        <insert_code>
           <file>examples/prologue.s</file>
       </insert_code>
    </kernel>
```

Then the main kernel which contains only one instruction is inserted:
```
    <kernel>
    <!--instruction part -->
        <!--allow instructions to be randomized in benchmarks-->
            <instruction>
 
                <operation>movapd</operation>
 
                <memory>
                    <register>
                        <name>r1</name>
                    </register>
                    <offset>0</offset>
                </memory>
 
                <register>
                    <phyName>%xmm</phyName>
                    <min>0</min>
                    <max>8</max>
                </register>
 
                <swap_after_unroll/>
            </instruction>
```
The kernel is not yet closed since there still is additional information.

### Instruction ###

The instruction in the kernel is a **movapd** instruction defined by the _operation_ node. Its operands are a memory and a register operand. Finally, there is a swap\_after\_unroll specifier to allow the tool to swap the operands after having performed the unroll pass. When using the swap\_after\_unroll specifier, the tool unrolls the kernel and for each instruction in the unrolled version, it creates two variants: the original order of operands and the swapped version.  Further information is given in [Chapter 4: Input Specification](MicroCreator_Chapter_4_Input_Specification.md)].

#### Memory Operand ####

The memory operand here contains a register name _r1_.  _r1_ is a logical name which is later updated to a physical x86 register during the register allocation pass.  The correspondence between logical names and physical names is defined by [MicroDetector](MicroDetector.md).

The offset informs the operand of its base offset.

#### Register Operand ####

A register operand, as opposed to a memory operand, uses a physical register name directly.  Of course, it is also possible to use a logical name but the example only illustrates the use of physical register names.

The base physical register name used here is a Xmm register.  Furthermore, during the unrolling process, the user might wish to use a different register, for example from xmm0 to xmm7 by putting xmm in the _phyName_ and then adding the _min/max_.

The numbering is updated at each iteration of the unroll.  Though the tool is not exactly _unrolling_ since it is modifying register allocation, it is a useful technique when the user wishes to obtain:

```
 #Unrolling, iteration 1 out of 6
 movapd 0(%rsi), %xmm0
 #Unrolling, iteration 2 out of 6
 movapd %xmm1, 16(%rsi)
 #Unrolling, iteration 3 out of 6
 movapd 32(%rsi), %xmm2
 #Unrolling, iteration 4 out of 6
 movapd 48(%rsi), %xmm3
 #Unrolling, iteration 5 out of 6
 movapd %xmm4, 64(%rsi)
 #Unrolling, iteration 6 out of 6
 movapd %xmm5, 80(%rsi)
```

  * Notes:
    * The example is a LSLLSS scheme where L represents a Load and S a Store
    * The numbers for the Xmm registers have increased at each new instruction

### Further Information for the Kernel ###

The example kernel contains more information about how the creator handles or modifies the kernel.

#### Unrolling ####

The example shows how to request an unroll factor from one to eight:

```
    <unrolling>
         <min>1</min>
         <max>8</max>
         <progress>1</progress>
    </unrolling>
```

The progress is, of course, the step between each different unrolling factor. A progress of two would generate the following factors: 1, 3, 5, 7.

#### Induction Variables ####

In the example, there are three induction variables:

  * One for the target and source address used by the load and store instructions

  * One for the iteration counter used as a return to the function

  * One for the loop counter which is reduced by two at every iteration (since the example is using movapd which loads and stores two doubles, the example needs to decrement by two at each iteration)

The first induction variable represents the target or source address:

```
    <induction>
        <register>
            <name>r1</name>
        </register>
        <increment>16</increment>
        <offset>16</offset>
        <not_affected_unroll/>
    </induction>
```

The register naming is also either logical or physical; in the current example, it is logical. The difference between increment and offset is whether the offset is used when handling unrolling of an instruction using the register or the increment is what is added at the end of the loop. In the example's case, the induction variable is not affected by the unroll. The increment will always be sixteen disregarding the unroll factor. [Chapter 4](MicroCreator_Chapter_4_Input_Specification#Induction_Node.md) provides further information about the options.

The second induction variable is simpler and the only notable difference is the use of a physical register:

```
    <induction>
         <register>
             <phyName>%eax</phyName>
         </register>
         <increment>1</increment>
         <not_affected_unroll/>
    </induction>
```

The last induction variable adds two new options:

```
    <induction>
         <register>
            <name>r0</name>
         </register>
         <increment>-2</increment>
         <not_affected_unroll/>
         <linked>
             <register>
                <name>r1</name>
             </register>
         </linked>
         <last_induction/>
    </induction>
```

  * The linked node means any modification of the stride chosen for the linked induction variable is copied for the current node

The last induction is used to force the code generator to have the induction variable as the last induction variable instruction of the kernel, which is useful when the branch instruction of the loop, generated after the induction variable instructions, is using the result of the last increment or decrement to decide whether to exit or not.

#### Branch Information ####

Finally, if a kernel wishes to be generated as a loop, it is necessary to provide the branch information of the loop:

```
        <branch_information>
            < label>L6</label>
            <test>jge</test>
        </branch_information>
```

The label is the label used for the loop and the test is the comparison instruction used for the branch instruction.

### Rest of the Input File ###

The rest of the file contains the epilogue insertion to the generated example files:
```
    <kernel>
        <insert_code>
           <file>examples/epilogue.s</file>
        </insert_code>
    </kernel>
```

And, finally, the hardware detector information:

```
    <hardware_detector>
        <execute>../microdetect/microdetect ../microdetect/data/args.c ./microdetect/output</execute>
        <information_file>../microdetect/output</information_file>
    </hardware_detector>
```

  * First, the _execute_ node explains how to execute the detector

  * Second, the _information\_file_ provides where the detector stores its output

[The hardware detector section](MicroCreator_Chapter_4_Input_Specification#hardware_detector.md) explains in greater detail the hardware\_detector node.

# Conclusion #

The tool itself uses an xml file and certain command line options to define its behavior. It can create as many benchmarks as the user wishes. It is entirely linked and associated to the [launcher](MicroLauncher.md) and [detector](MicroDetector.md) tools.

# Index Navigation #

The following chapter is part of the [MicroCreator Manual](MicroCreator.md).

The previous chapter is [Chapter 2: Installation and First Use](MicroCreator_Chapter_2_Installation.md).

The next chapter is [Chapter 4: Input Specification](MicroCreator_Chapter_4_Input_Specification.md).