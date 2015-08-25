### Index Navigation ###

This page is of the [MicroCreator Manual](MicroCreator.md) and, more specifically, [Chapter 4: Input Specification](MicroCreator_Chapter_4_Input_Specification.md).

# Introduction #

Unrolling is one of the central elements of MicroCreator. The concept of unrolling involves repeating the iteration of a loop without repeating the loop control code. The outcome increases Instruction Level Parallelism (ILP) and reduces loop control related instructions and branch prediction misses. There are certain differences between general compiler unrolling and what is performed in MicroCreator. The page presents these differences and explains the core elements of what happens when MicroCreator unrolls a kernel.

The rest of the page uses the same example to show the full process of unrolling. It defines a load instruction as unrolled one to eight times. With such an input, MicroCreator generates eight microbenchmark programs.

```
<pre>
<kernel>
  <instruction>
     <operation>movaps</operation>

     <register>
       <phyName>%xmm</phyName>
       <min>0</min>
       <max>8</max>
     </register>

     <memory>
       <register>
         <name>r0</name>
       </register>
       <offset>0</offset>
     </memory>

     <unrolling>
        <min>1</min>
        <max>8</max>
     </unrolling>
  </instruction>
</kernel>
</pre>
```

# Basics #

MicroCreator unrolls [kernels](MicroCreator_Chapter_4_Input_Specification#Kernel.md), nodes representing basic blocks. MicroCreator can unroll any kernel, whether it represents a loop or not. The outcome is exactly the same, which is not the case for generic compilers.

Without unrolling, the generated code is:
```
 movaps %xmm0, 0(r0)
```

The first step when unrolling a kernel is to repeat the kernel's instructions. The number of repetitions is called the unrolling factor. The rest of the document presents, as an example, an unrolling factor of three. After the first stage, the tool transforms the code into:
```
 movaps %xmm0, 0(r0)
 movaps %xmm0, 0(r0)
 movaps %xmm0, 0(r0)
```

# Register Names #

In the example, the registers Xmm are defined as having a minimum and maximum value used for modifying the registers for the unrolling. If a register contains a minimal and maximum value, each copied iteration retrieves a different index. Therefore, instead of only having the xmm0 and [r0](https://code.google.com/p/microperf/source/detail?r=0) registers, MicroCreator generates:
```
 movaps %xmm0, 0(r0)
 movaps %xmm1, 0(r0)
 movaps %xmm2, 0(r0)
```

MicroCreator can produce the same numbering system for virtual names. If the register node for [r0](https://code.google.com/p/microperf/source/detail?r=0) was instead:
```
     <memory>
       <register>
         <name>r</name>
         <min>0</min>
         <max>8</max>
       </register>
       <offset>0</offset>
     </memory>
```

It would generate:
```
 movaps %xmm0, 0(r0)
 movaps %xmm1, 0(r1)
 movaps %xmm2, 0(r2)
```

## Overflow ##

If the maximum is reached, the numbering system starts at zero again. Therefore, in the case of an unrolling factor of ten, MicroCreator generates:
```
 movaps %xmm0, 0(r0)
 movaps %xmm1, 0(r1)
 movaps %xmm2, 0(r2)
 movaps %xmm3, 0(r3)
 movaps %xmm4, 0(r4)
 movaps %xmm5, 0(r5)
 movaps %xmm6, 0(r6)
 movaps %xmm7, 0(r7)
 movaps %xmm8, 0(r8)
 
 #Rolling back the register numbering system
 movaps %xmm0, 0(r0)
```

The register numbering system is done to reduce the register pressure during the kernel's execution.

# Offsets #

When unrolling, it is important to modify the offsets of the memory operations. Unrolling does not change the semantics of the program, it simply expands the code base and reduces loop control code. Therefore, between a loop containing a load from [r0](https://code.google.com/p/microperf/source/detail?r=0) to xmm0 and an incrementation of sixteen:
```
 movaps %xmm0, 0(r0)
 add r0, 16
```

Unrolling the loop three times without modifying the offsets generates:
```
 movaps %xmm0, 0(r0)
 movaps %xmm0, 0(r0)
 movaps %xmm0, 0(r0)
 add r0, 16
```

Now, the program loads three times the data into the same register, which is no longer the same program from a semantic point of view. In order to only load the data once, the offsets must be updated:
```
 movaps %xmm0, 0(r0)
 movaps %xmm0, 16(r0)
 movaps %xmm0, 32(r0)
 add r0, 16
```

Finally, MicroCreator generates:
```
 movaps %xmm0, 0(r0)
 movaps %xmm1, 16(r0)
 movaps %xmm2, 32(r0)
 add r0, 16
```

# Induction Variables #

Finally, the induction variables must be updated. Consider the following loop structure:
```
 for (i = 0; i < N; i++)
   body
```

The loop unrolled twice produces:
```
 for (i = 0; i < N; i + = 2)
   first copy of body
   second copy of body
```

The increment or decrement of each induction variable must be multiplied by the unrolling factor. In our case, MicroCreator finally generates:
```
 movaps %xmm0, 0(r0)
 movaps %xmm1, 16(r0)
 movaps %xmm2, 32(r0)
 add r0, 48
```

# Index Navigation #

This page is of the [MicroCreator Manual](MicroCreator.md) and, more specifically, [Chapter 4: Input Specification](MicroCreator_Chapter_4_Input_Specification.md)