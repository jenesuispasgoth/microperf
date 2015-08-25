### Index Navigation ###

This chapter is part of the [MicroCreator Manual](MicroCreator.md).

The previous chapter is [Chapter 3: General Usage](MicroCreator_Chapter_3_General_Usage.md).

The next chapter is [Chapter 5: Error Messages](MicroCreator_Chapter_5_Error_Messages.md).

# General Navigation #

The page specifies the input file format and the existing options for each element, etc.  With the number of nodes increasing, to ease the navigation throughout the page, a table index is provided.  Each cell represents a node and the columns represents the level at which a node may appear and in which node it does.

<table border='1'>
<tr>
<th>Level 1</th><th>Level 2</th><th>Level 3</th><th>Level 4</th>
</tr>

<tr>
<td><a href='MicroCreator_Chapter_4_Input_Specification#Description.md'>Description</a></td>
<td><a href='MicroCreator_Chapter_4_Input_Specification#Benchmark_amount.md'>Benchmark_amount</a></td>
</tr>

<tr>
<td><a href='MicroCreator_Chapter_4_Input_Specification#Kernel.md'>Kernel</a></td>
<td><a href='MicroCreator_Chapter_4_Input_Specification#Branch_Information.md'>Branch_Information</a></td>
</tr>

<tr>
<td><a href='MicroCreator_Chapter_4_Input_Specification#Kernel.md'>Kernel</a></td>
</tr>

<tr>
<td><a href='MicroCreator_Chapter_4_Input_Specification#Induction.md'>Induction</a></td>
</tr>

<tr>
<td><a href='MicroCreator_Chapter_4_Input_Specification#Insert_Code.md'>Insert_Code</a></td>
<td><a href='MicroCreator_Chapter_4_Input_Specification#Instruction.md'>Instruction</a></td>
</tr>

<tr>
<td><a href='MicroCreator_Chapter_4_Input_Specification#File.md'>File</a></td>
</tr>

<tr>
<td><a href='MicroCreator_Chapter_4_Input_Specification#Instruction.md'>Instruction</a></td>
<td><a href='MicroCreator_Chapter_4_Input_Specification#Choose_Operation_After_Unroll.md'>Choose_Operation_After_Unroll</a></td>
</tr>

<tr>
<td><a href='MicroCreator_Chapter_4_Input_Specification#Choose_Operation_Before_Unroll.md'>Choose_Operation_Before_Unroll</a></td>
</tr>

<tr>
<td><a href='MicroCreator_Chapter_4_Input_Specification#Immediate.md'>Immediate</a></td>
</tr>

<tr>
<td><a href='MicroCreator_Chapter_4_Input_Specification#Immediate_After_Unroll.md'>Immediate_After_Unroll</a></td>
</tr>

<tr>
<td><a href='MicroCreator_Chapter_4_Input_Specification#Immediate_Before_Unroll.md'>Immediate_Before_Unroll</a></td>
</tr>

<tr>
<td><a href='MicroCreator_Chapter_4_Input_Specification#Indirect_Memory.md'>Indirect_Memory</a></td>
</tr>

<tr>
<td><a href='MicroCreator_Chapter_4_Input_Specification#Memory.md'>Memory</a></td>
</tr>

<tr>
<td><a href='MicroCreator_Chapter_4_Input_Specification#Operation.md'>Operation</a></td>
</tr>

<tr>
<td><a href='MicroCreator_Chapter_4_Input_Specification#Register.md'>Register</a></td>
</tr>

<tr>
<td><a href='MicroCreator_Chapter_4_Input_Specification#Repetition.md'>Repetition</a></td>
</tr>

<tr>
<td><a href='MicroCreator_Chapter_4_Input_Specification#Swap_After_Unroll.md'>Swap_After_Unroll</a></td>
</tr>

<tr>
<td><a href='MicroCreator_Chapter_4_Input_Specification#Swap_Before_Unroll.md'>Swap_Before_Unroll</a></td>
</tr>

<tr>
<td><a href='MicroCreator_Chapter_4_Input_Specification#Loop_Info.md'>Loop_Info</a></td>
</tr>

<tr>
<td><a href='MicroCreator_Chapter_4_Input_Specification#Randomize.md'>Randomize</a></td>
</tr>

<tr>
<td><a href='MicroCreator_Chapter_4_Input_Specification#Unrolling.md'>Unrolling</a></td>
</tr>

<tr>
<td><a href='MicroCreator_Chapter_4_Input_Specification#Verbose.md'>Verbose</a></td>
</tr>


</table>

# Description Node #

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

The description node is the root node of the input containing no special options.  The purpose of the description node is to contain the rest of the  nodes: [kernels](MicroCreator_Chapter_4_Input_Specification#Kernel.md), [code insertions](MicroCreator_Chapter_4_Input_Specification#Insert_code.md), etc.

# Kernel #

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

The kernel node is one of the most important nodes since it allows the definition of which instructions are going to be generated, how to handle unrolling, etc.  The compiler community defines a basic block as a series of instructions with one instruction flow entry and a single instruction flow exit.

# Instruction #

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

Instruction is the basic node to define an instruction in assembly form.  It should contain the name of the operation and its operands.  There are also a few nodes, such as choose\_operation\_before\_unroll, which help define how to generate or modify the instruction generation process via various MicroCreator passes.

# Choose Operation Before Unroll #

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

The node is contained in a [Instruction](MicroCreator_Chapter_4_Input_Specification#Instruction.md) node.  The default behavior if the instruction node contains multiple operation nodes is to perform the choice before the unrolling.

# Choose Operation After Unroll #

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

When the user inserts the choose\_operation\_after\_unroll node in the instruction, and having defined multiple operation nodes, the choice of which operation to use is performed after the unrolling.

# Immediate #

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

An immediate operand defines any numerical value such as one, five, or forty-two.  However, in certain cases, the user might require trying various values for the immediate value.  If the user wants to generate programs which each use one of the values one, two, three, four, five or six.  As a result, there are two variations of the operand: a single value or a range of values.

Accordingly, the immediate operand can contain:

  * A single value node and no min, max, or progress nodes

  * Minimum, maximum, and progress values

Examples are:

  * Single:
```
 <immediate>
   <value>5</value>
 </immediate>
```
  * Range:

Generate every value between 1 and 1024:

```
 <immediate>
   <min>1</min>
   <max>1024</max>
 </immediate>
```

Generate every other value between 1 and 1024:

```
 <immediate>
   <min>1</min>
   <max>1024</max>
   <progress>2</progress>
 </immediate>
```

  * **Note** Errors regarding the use of options are available in [Chapter 5](MicroCreator_Chapter_5_Error_Messages.md).

# Immediate Before Unroll #

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

The immediate\_before\_unroll modifies the moment where an immediate value is selected.  When the user provides a range of values, MicroCreator either generates the variations of values before the unrolling process or after.  The immediate\_before\_unroll performs the selection before.

# Immediate After Unroll #

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

The immediate\_after\_unroll modifies the moment where an immediate value is selected.  When the user provides a range of values, MicroCreator either generates the variations of values before the unrolling process or after.  The immediate\_before\_unroll performs the selection after.

# Indirect Memory #

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

MicroCreator generates the following assembly code with an indirect memory operand:

```
  movaps 0(%rsi, %rdx, 8), %xmm0        #A store instruction
```

The first operand of the instruction is the indirect memory operand.  The zero is the offset, rsi is the base register, or base address, rdx is the index register and eight is the multiplier.

The address calculated for the store instruction is:

```
 address = 0 + %rsi + %rdx * 8
```

For MicroCreator, the corresponding node contains two normal register operands:

  * An index register

  * A base register

The node also can contain an offse' node and a multiplier node.

The XML code used to generate the assembly example above is:

```
  <indirect_memory>
    <base>
     <name>r1</name>
    </base>
    <index>
     <name>r2</name>
    </index>
    <multiplier>8</multiplier>
    <offset>0</offset>
 </indirect_memory>
```

  * Remember:
  * [R1](https://code.google.com/p/microperf/source/detail?r=1) and [r2](https://code.google.com/p/microperf/source/detail?r=2) are logical register names and are later transformed by the [detector tool](MicroDetector.md) into valid physical register names
  * A register node contains either a name or a phyName to define respectively a logical or a physical name

# Memory #

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

As opposed to the [indirect\_memory operand](MicroCreator_Chapter_4_Input_Specification#indirect_memory.md), the operand is directly an address register with an offset such as:

```
 0(%rdi)
```

To create a memory operand, define:

  * A [register operand](MicroCreator_Chapter_4_Input_Specification#indirect_memory.md)

  * An offset node with the integer value

The following XML code generates the operand 0(%rdi):

```
 <memory>
   <register>
     <phyName>%rdi</phyName>
   </register>
   <offset>0</offset>
 </memory>
```

# Operation #

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

The operation is generally the name of the intended instruction:

```
 <operation>movss</operation>
```

It is also possible to define multiple operations and generate programs with different operation choices.  Imagine a program generation requiring the comparison between movss and movsd:

```
 <instruction>
   <operation>movss</operation>
   <operation>movsd</operation>
 </instruction>
```

The previous instruction node generates one program with the instruction with an operation movss and another program with the instruction created with movsd.

However, as the [unroll pass](MicroCreator_Chapter_4_Input_Specification_Unrolling.md) is a bit particular for MicroCreator, there is a before and after unrolling option.

Before unrolling, each program either has movss or movsd for the operation of the instructions.  On the other hand, after unrolling, MicroCreator generates any combination of the two operations.

If the unroll factor is 2, choosing the operation before leads to two different programs:

  * Movss, movss

  * Movsd, movsd

However, choosing after the unrolling adds two additional programs:

  * Movss, movsd

  * Movsd, movss

# Register #

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

A register is the base operand for an instruction since it represents a register in the assembly file.

The options define whether it is a logical named register, defined later by the [MicroDetector tool](MicroDetector.md), or it is a physical named register.

To define a logical name:

```
 <register>
   <name>r0</name>
 </register>
```

To describe a physical register name:
```
 <register>
   <phyName>%eax</phyName>
 </register>
```

Additionally, sometimes the user might want to have different registers after unrolling.  MicroCreator permits numerical values by defining a minimum, a maximum, or a progress node:

```
 <register>
   <phyName>%xmm</phyName>
   <min>0</min>
   <max>8</max>
 </register>
```

Such a register definition generates registers %xmm0, %xmm1, . . ., and %xmm8 after unrolling.  Further information is given on the [unrolling page](MicroCreator_Chapter_4_Input_Specification_Unrolling.md).

# Repetition #

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

Sometimes, the user wants to have repetitions of a given instruction.  The node, included in an instruction, informs the tool of the minimum and maximum number of repetitions:

```
 <repetition>
   <min>1</min>
   <max>5</max>
 </repetition>
```

# Swap Before Unroll #

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

Swapping operands generates two versions of the instruction.  There are two possibilities, swapping the operands before or after unrolling.

Using the swap\_before\_unroll node, MicroCreator swaps the operands before unrolling.  If the user defines an instruction **op A, B**, with an unroll factor of two, the tool generates two programs:

```
 op A, B
 op A, B

 op B, A
 op B, A
```

# Swap After Unroll #

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

Swapping operands generates two versions of the instruction.  There are two possibilities, swapping the operands before or after unrolling.

Using the swap\_after\_unroll node, MicroCreator swaps the operands after unrolling.  If the user defines an instruction **op A, B**, with an unroll factor of two, the tool generates four programs:

```
 op A, B
 op A, B

 op A, B
 op B, A

 op B, A
 op A, B

 op B, A
 op B, A
```

# Insert Code #

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

When inserting code directly, the insert\_code node is used.  There are two options: either the user inserts an instruction verbatim or from a particular file.

## Instruction ##

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

When inserting only one instruction, MicroCreator supports the instruction node:

```
        <insert_code>
            <instruction>xor %xmm0, %xmm0</instruction>
        </insert_code>
```

Such an insert\_code generates the assembly instruction xor %xmm0, %xmm0.

## File ##

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

The second variation copy-pastes code from a file directly into the generated code.  For instance:
```
    <a first kernel/>
 
    <kernel>
        <insert_code>examples/prologue.s</insert_code>
    </kernel>
    
    <another kernel/>
```

The kernel would then copy paste the code from examples/prologue.s between the code generated for the two other kernels.

# Randomize #

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

Using such a node randomizes the generation of the instruction scheduling.  The system generates every possible schedule of a kernel.  If the kernel contains an instruction A and an instruction B, the tool generates the following schedules:

  * A - B

  * B - A

# Unrolling #

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

Unrolling allows to generate various versions of the kernel by generating multiple unroll factors.  The internal nodes are:

  * Min: the minimum value of the unrolling factor

  * Max: the maximum value of the unrolling factor

  * Progress: the step between unrolling factors

To inform the generator to create eight versions using an unroll factor of one to eight:

```
        <unrolling>
            <min>1</min>
            <max>8</max>
            <progress>1</progress>
        </unrolling>
```

A progress of two would generate factors one, three, five, and seven.

For further information, please refer to the [unrolling page](MicroCreator_Chapter_4_Input_Specification_Unrolling.md).

# Induction #

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

An induction variable is a variable which is updated at each iteration of the loop.

## Comment ##
  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

The comment option allows the user to specify a comment to add in the assembly output.

```
    <induction>
      <comment>Induction_status:Success</comment>
      <register>
        <name>r1</name>
      </register>
      <increment>64</increment>
    </induction>
```

creates:

```
 add $64, %rsi # Induction_status:Success
```

## Stride ##

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

The stride option allows the user to define the minimum and maximum stride for an induction variable.  If no stride is defined, the stride used is one.

To define different strides, the user can add two nodes: a minimum and a maximum.  To define a stride variation from five to ten:

```
 <stride>
   <min>5</min>
   <max>10</max>
 </stride>
```

## Register ##

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

The definition of register has already been defined [above](MicroCreator_Chapter_4_Input_Specification#register.md).  The induction variable uses the same definition as a generic register in an instruction.

## Not Affected Unroll ##

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

When the unrolling passes chooses a factor, it updates the induction variable's increment or decrement value by the same factor.

Thus, consider:

```
 for (i = 0; i < 10; i++)
```

If the loop is unrolled twice, it becomes:

```
 for (i = 0; i < 10; i + = 2)
```

The option renders the induction variable's update independent of the unrolling factor, thus never modified.

## Last Induction ##

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

The last\_induction node defines which induction variable is the last induction variable to be updated.  It is useful when the result of the arithmetic operation involving the variable is used to decide whether to branch or not.

Consider:

```
 <induction>
   #Induction variable A
   <last_induction/>
 <induction>
 
 <induction>
   #Induction variable B
 <induction>
```

The XML code generates:

```
 Update of induction variable B
 Update of induction variable A
```

## Offset ##

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

The offset of an induction variable determines by how much it should be updated in the case of unrolling.

Consider the following code:

```
  movaps 0(%rsi), %xmm0    # Load 16 bytes to xmm0
  add $16, %rsi            # Add 16 to rsi to prepare for the next iteration
```

Now, if MicroCreator unrolls the code without a defined offset, it generates:

```
  movaps 0(%rsi), %xmm0    # Load 16 bytes to xmm0
  movaps 0(%rsi), %xmm1    # Load 16 bytes to xmm1
  add $32, %rsi            # Add 32 to rsi to prepare for the next iteration
```

Such a code loads the same value into both Xmm variables.

If the user defines an offset of sixteen in the induction variable with:

```
 <offset>16</offset>
```

MicroCreator generates:
```
  movaps 0(%rsi), %xmm0    # Load 16 bytes to xmm0
  movaps 16(%rsi), %xmm1   # Load 16 bytes to xmm1
  add $32, %rsi            # Add 32 to rsi to prepare for the next iteration
```

## Increment ##

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

Increment provides the value which is added or subtracted to the induction variable at each iteration.

## Linked ##

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

Linked allows the system to be strided in the same way as the linked induction variable.  In certain cases, it is important to have two induction variables strided the same way.

# Branch Information #

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

When the kernel node is used to define a loop, it needs to contain the branch\_information node.  A branch\_information node should contain a label to define the loop label and test is the test instruction to be used:

```
 <branch_information>
    < label>L6</label>
    <test>jge</test>
 </branch_information>
```

It is the user's job to make sure the label is unused in the rest of the file.  Otherwise, the behavior is undefined.

# Loop Info #

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

The node is used to define information for the OpenMP loop structure.  It contains three possible sub-nodes:

  * Induction: which represents the name of the [induction operand](MicroCreator_Chapter_4_Input_Specification#induction.md) of the loop

  * [count\_iteration](MicroCreator_Chapter_4_Input_Specification#count_iteration.md): which represents the count information for handling the loop

  * [omp\_option](MicroCreator_Chapter_4_Input_Specification#omp_option.md): which provides what OMP options the user requires

## Count Iteration ##

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

The count\_iteration node provides the information necessary for handling a loop in an OpenMP code.  It contains three possible sub-nodes:

  * Induction: which represents the name of the [induction operand](MicroCreator_Chapter_4_Input_Specification#induction.md) of the loop

  * Min: the initial value for the loop

  * Max: the last value for the loop

A sample:
```
            <count_iteration>
                <induction>i</induction>
                <min>0</min>
                <max>100</max>
            </count_iteration>
```

generates:
```
 for (i = 0; i < 100; i++)
```

# Omp Option #

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

The node provides the user options for the OMP pragma.  It currently contains three OpenMP possibilities private, shared, and first\_private.

The following option:
```
            <omp_option>
                <shared>r0</shared>
                <private>x0</private>
            </omp_option>
```

generates:

```
    #pragma omp parallel for shared (r0) private (x0)
```

## Benchmark Amount ##

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

The node defines the maximal number of benchmarks created by the tool:

```
 <benchmark_amount>100</benchmark_amount>
```

## Hardware Detector ##

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

The hardware detection is the link from the creation tool to the [detector](MicroDetector.md) tool.  The detector tool allows the register allocation system to go from virtual registers to physical registers.

The node takes two internal nodes:

  * Execute: the string used to execute the detection tool and what command-line options should be used

  * Information\_file: the output generated by the detection tool

An example is:

```
    <hardware_detector>
        <execute>../microdetect/microdetect ../microdetect/data/args.c ../microdetect/output</execute>
        <information_file>../microdetect/output</information_file>
    </hardware_detector>
```

## Verbose ##

  * [Return to the Description index table](MicroCreator_Chapter_4_Input_Specification#General_Navigation.md)

Adding a verbose node into the description tool sets the system into a verbosis tool.  The difference between the normal silent mode and the verbosis mode is it creates an output file for each pass, giving extra information on what has happened.

  * ote:**be wary for a huge generation, the number of files generated for the verbosis can become large**

# Index Navigation #

This chapter is part of the [MicroCreator Manual](MicroCreator.md).

The previous chapter is [Chapter 3: General Usage](MicroCreator_Chapter_3_General_Usage.md).

The next chapter is [Chapter 5: Error Messages](MicroCreator_Chapter_5_Error_Messages.md).