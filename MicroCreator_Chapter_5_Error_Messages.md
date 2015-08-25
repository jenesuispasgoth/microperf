### Index Navigation ###

This chapter is part of the [MicroCreator Manual](MicroCreator.md).

The previous chapter is [Chapter 4: Input Specification](MicroCreator_Chapter_4_Input_Specification.md).

The next chapter is [Chapter 6: The Plugin System](MicroCreator_Chapter_6_The_Plugin_System.md).

# Introduction #

Most error messages are found in the log file automatically created called Log.txt. When the tool is not doing what is expected, look in log file and search for lines starting with Warning or Error.

For any questions or messages which do not appear on this list, please contact the developers for additional information.

# General Messages #

## Input File is Empty ##

```
 Error_Input file is empty
```

The input file provided by the user is empty.

## Parsing Error ##

```
 XML_parsing error
```

There was an error with the parsing. Additional information is generally provided to help understand the issue.

# Parsing XML Messages #

Most warning or error messages are produced during the XML parsing, generally provoking an incorrect code generation. The error is corrected by fixing the issue with the input file.

## Description Node Missing ##

```
 Error_missing description node
```

MicroCreator generates the message when the root node of the XML file is not called [Node description](MicroCreator_Chapter_4_Input_Specification#Description.md).

Note: the tool considers only one root node. If a second root node exists in the file, it is ignored and only the first root node is processed.

## Branch Messages ##

### Branch Information Node Error, Information Missing ###

```
 Branch information node error, information missing
```

A [branch information node](MicroCreator_Chapter_4_Input_Specification#Branch_Information.md) must contain a label and a test node; if it does not, the message is displayed.

### Missing Label ###

```
 XML_Warning_missing Label value
```

MicroCreator generates the message when the user has provided an empty label node in a [branch node](MicroCreator_Chapter_4_Input_Specification#Branch_Information.md) such as:

```
 < label >
 </ label >
```

### Missing Instruction ###

```
 XML_missing instruction of the label
```

The tool outputs the message when the user forgot to provide the instruction to use in a test node in a [branch node](MicroCreator_Chapter_4_Input_Specification#Branch_Information.md).

## File Not Found ##

```
 Could not find file_test.s for InsertCode
```

The error message appears when the file provided to the [insert\_code node](MicroCreator_Chapter_4_Input_Specification#Insert_Code.md) is not available for reading or does not exist.

## Hardware Detector ##

```
 XML_Error_missing information_file node in hardware_detector
```

A [hardware detector node](MicroCreator_Chapter_4_Input_Specification#Hardware_Detector.md) must at least contain an information\_file node.

## Immediate Wrong Values ##

```
 XML_Warning_immediate value _wrong parameters
```

MicroCreator generates the message  when, in an [immediate node](MicroCreator_Chapter_4_Input_Specification#Immediate.md), the provided values are incorrect. Incorrect values include:

  * Minimal value greater than the maximal one

  * Minimal or maximal value provided at the same time as a constant value

  * Progress is negative

## IndirectMemoryOperand ##

```
 Warning_Register r0 is an induction variable and should not be in the IndirectMemoryOperand
```

The register provided to the [indirect\_memory node](MicroCreator_Chapter_4_Input_Specification#Indirect_Memory.md) is actually an induction variable not supported by MicroCreator.

## Induction variable ##

```
 XML_Error_induction variable_virtual name_r0, physical name_not found, is it declared after line_
```

The user tried to link two [induction](MicroCreator_Chapter_4_Input_Specification#Induction.md) variables. A common case is when the linked variable name is not found because the provided name is wrong or appears later in the XML file.

## Instruction Repetition ##

```
 XML_Error_instruction repetition not enough/wrong parameters at:
```

The user provided incorrect parameters in the [instruction repetition](MicroCreator_Chapter_4_Input_Specification#Repetition.md) node. Generally, such an error occurs because the provided minimal value is greater than the maximal value or the progress is negative.

## Loop Wrong Parameters ##

```
 XML_Warning_For loop not enough/wrong parameters
```

MicroCreator generates the message when the minimal value is greater than the maximal value in a [count\_iteration node](MicroCreator_Chapter_4_Input_Specification#Count_Iteration.md).

## MDL Schedule ##

```
 XML_Warning_MDLBasicSchedule _wrong parameters at:
```

The message appears when, in an [MDLBasicSchedule node](MicroCreator_Chapter_4_Input_Specification#MDLBasicSchedule.md), the provided values are incorrect. Reasons why the message is displayed include:

  * Minimal value greater than the maximal one

  * Minimal or maximal value provided at the same time as a constant value

  * Progress is negative

## Operand Name Missing ##

```
 Error_Node in register operand hasn't a name node at line:
```

MicroCreator outputs the message when the user forgot to provide a [register](MicroCreator_Chapter_4_Input_Specification#Register.md) name for the node.

## Unroll ##

```
 XML_Warning_unroll not enough/wrong parameters at:
```

The message appears when there is a problem with the [unrolling](MicroCreator_Chapter_4_Input_Specification#Unrolling.md) parameters such as the minimum value is greater than the maximum value or the progress is negative.

## Unsupported Name X in Y ##

```
 XML_Warning_Unsupported name X in Y
```

When MicroCreator detects an invalid node, the message is printed. For example, if the user wrote unrollIt instead of unrolling.

```
 <unrollIt>
    <min>5</min>
    <max>10</max>
 </unrollIt>
```

# Plugin System #

The plugin system also generates a few error messages that help the user understand how to fix the issues.

### File Not Found ###

```
 Plugin file not found
```

The message appears when the plugin file specified by the command-line is not found.

### Remove Unexisting Pass ###

```
 Plugin requests to remove unexisting pass <Name>
```

The pass name provided by the user when removing a pass is not found.

### Add Before or After Unexisting Pass ###

```
 Plugin requests to add pass after pass <Name>, but <Name> does not exist_pass will be added at the end
 Plugin requests to add pass before pass <Name>, but <Name> does not exist_pass will be added at the start
```

The pass name provided by the user when adding before or after a pass is not found.

# Index Navigation #

This chapter is part of the [MicroCreator Manual](MicroCreator.md).

The previous chapter is [Chapter 4: Input Specification](MicroCreator_Chapter_4_Input_Specification.md).

The next chapter is [Chapter 6: The Plugin System](MicroCreator_Chapter_6_The_Plugin_System.md).