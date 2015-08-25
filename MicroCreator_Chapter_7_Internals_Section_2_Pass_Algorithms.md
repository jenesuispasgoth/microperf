### Index Navigation ###

This section is part of [Chapter 7](MicroCreator_Chapter_7_Internals.md) in the [MicroCreator Manual](MicroCreator.md).

The previous section is [Section 1: General](MicroCreator_Chapter_7_Internals_Section_1_General.md).

The next section is [Section 3: Plugin System](MicroCreator_Chapter_7_Internals_Section_3_Plugin_System.md).

# Introduction #

MicroCreator is a source-to-source compiler containing a certain number of passes. A pass in MicroCreator represents any algorithm taking as input a kernel and outputing a vector of kernels. To be more precise, the passes are defined in the subdirectory Passes and have an entry function defined by the base class Pass:

```
 virtual std::vector <PassElement *> *entry (PassElement *pe, Description *desc) const;
```

A PassElement contains a ernel and a pass descriptor.

# Passes #

Currently, the following passes are defined as general passes:

  * Statement scheduling

  * Induction selection

  * Immediate selection before unroll

  * Operand swap before unroll

  * Operation choose before unroll

  * Stride selection

  * Pass unroll

  * Operation choose after unroll

  * Operand swap after unroll

  * Immediate selection after unroll

  * Induction insertion

  * Register allocation

  * Code generation


The list is obtained by looking in the Pass directory or by using the ''-l'' command-line option. Below is information about each pass.

## StatementSelection ##

The StatementSelelection pass is probably the most important pass of the program because it determines which statements of the original input file are added into the final kernel and, more importantly, how.

### Algorithm ###

The algorithm is relatively straight-forward:

  * Go through each kernel:
    * Determine whether the user wishes to randomize the statements
      * **If not, for each statement:
        *****Determine whether or not the user wishes to repeat the statement
          *** If so, repeat the statement

As it is seen, there are two big portions to the algorithm: the random generator and the repetition system. The random generator generates every possible permutation of the kernel's statements. The repetition system repeats a statement a set number of times.

## ImmediateSelection ##

The Immediate Selection pass chooses a value for any immediate variable defined as a range.

The algorithm is straight forward: it traverses the kernel and finds any immediate operand defined as a range. If one is found, the kernel is duplicated with an immediate variable instantiated with one value of the range.

## InductionSelection ##

The Induction Selection pass links every instruction's register operands with the kernel's induction variables. The algorithm performs a classic walk through the instruction tree and calls the operand's handleInductionVariables function.

## OperandSwap ##

OperandSwap creates variations of an instruction by swapping the order of the operands. The algorithm is a simple tree walker checking whether or not the instruction requests an operand swap. If it does, the operands are swapped.

An instruction can request a swap before or after unrolling. Therefore, the OperandSwap pass also has an internal boolean variable to determine if it should swap before the unrolling or after. If both the instruction and the OperandSwap want to swap at the same time, the pass generates a new kernel with the new instruction. The only difference between both kernels is the instruction which has swapped operands.

## OperationChoose ##

The OperationChoose pass generates variations of a kernel containing multi-operation instructions. An instruction may contain more than one operation. In these cases, the pass generates a separate kernel for each operation.
The pass is actually used before and after the unrolling pass, a boolean value is used to determine in which scenario the tool is.

## StrideSelection ##

StrideSelection selects the stride for induction variables having multiple requested strides. The pass generates a kernel for each combination of strides for each induction variable. To create every variation, the algorithm goes through the induction variables and creates an array to choose each possible stride. It then goes through every combination and creates a copy of the original benchmark.

## PassUnroll ##

PassUnroll is the unrolling pass which is the central pass for MicroCreator. A lot of studies rely on testing the effect of unrolling and analyze its benefits or performance degradation. In the case of MicroCreator, the pass is important to provide the users with great flexibility. The [unrolling page](MicroCreator_Chapter_4_Input_Specification_Unrolling.md) provides a full explanation of how unrolling effects the kernel.

## InductionInsertion ##

The InductionInsertion pass inserts an instruction for each induction variable of the kernel. An induction variable is incremented or decremented at each iteration of the loop. The induction variables are not defined as instructions in the kernels before the pass. During the execution of the pass, the system creates the increment or decrement instruction for each induction variable and inserts the instruction into the kernel's code.

## RegisterAllocation ##

MicroCreator defines register names as being virtual or physical. When a register name is physical, it means MicroCreator simply copies the register name into the generated instruction. When the name is virtual, MicroCreator must transform it into a physical register name. The association between names is provided by [MicroDetector](MicroDetector.md). RegisterAllocation is the pass performing the association.

## OMPCode ##

When creating OpenMP code, there are certain code elements needing to be generated depending on the generated code. The OMPCode pass handles the code creation. For example, the pass generates a certain number of local variables required by the kernel's generation.

## CodeGeneration ##

The CodeGeneration pass is one of the last passes and creates the generated benchmark program files. The algorithm is divided into five steps that each generate:

**The [prologue](MicroCreator_Chapter_4_Input_Specification#prologue.md) if one is provided**

**The label, if provided**

**The text version of the kernel**

**The jump, if provided**

**The [epilogue](MicroCreator_Chapter_4_Input_Specification#epilogue.md) if one is provided**

The generation process goes through each statement and calls the getString function. The only distinction performed during the algorithm is whether the tool is used to generate assembly code or C-code. If it is C-code, the instructions are embedded into asm directives such as:

```
 asm volatile ("movss (%rdi), %xmm0");
```

In assembly form, MicroCreator only generates:
```
 movss (%rdi), %xmm0
```

# Index Navigation #

This section is part of [Chapter 7](MicroCreator_Chapter_7_Internals.md) in the [MicroCreator Manual](MicroCreator.md).

The previous section is [Section 1: General](MicroCreator_Chapter_7_Internals_Section_1_General.md).

The next section is [Section 3: Plugin System](MicroCreator_Chapter_7_Internals_Section_3_Plugin_System.md).