# ARM UAL Guide

## Syntax

Each assembly source line consists of *words* separated by whitespace, `,` or `;`. Additional spaces or tabs between or before words are ignored. Unlike many assemblers initial space has no significance. Each line startes, optionally, with a label. If the second word is a valid op-code then the first word will be interpreted as a label, otherwise it must be an op-code.


```
[Label] OPCODE operand1, ... , operandn [ ; comment ]
```



## [Data Processing Instructions](https://tomcl.github.io/visual2.github.io/data-processing.html)

## [Single Memory Transfer Instructions](https://tomcl.github.io/visual2.github.io/memory-transfer.html)

## [Multiple Register Memory Transfer Instructions](https://tomcl.github.io/visual2.github.io/multiple-register-transfer.html)

## [Full Instruction List](https://tomcl.github.io/visual2.github.io/list.html#instructions)


## Introduction

![](visual-screen.jpg)

Each line of a VisUAL program defines part of an ARM CPU's initial memory. **Instruction lines** define the contents of a **code memory area**, starting from memory address 0. **Data definition lines** define data locations used by the program and are placed automatically in memory addresses after this.

Any line can be **labelled** with a symbol that can be used to reference the memory address into which the line is placed. For advanced users **EQU lines** define additional symbols like labels.

When writing assembler programs you can use instructions (executed sequentially) that refer to data locations you have defined, or that read and write uninitialised memory you have not defined.

The VisUAL simulator emulates an ARM CPU by sequentially executing instructions. A single assembler file and will always start from the first defined instruction in the file. Simulation terminates at the end of the file, or if an `END` line is executed.
