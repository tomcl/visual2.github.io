## UAL Instruction List

### Instruction Conditions

All UAL op-codes except directives and pseudo-instructions are can have an execution condition attached, see [Instruction Conditions](https://github.com/tomcl/visual2.github.io/blob/master/conditions.md).

### Data Processing Instructions

These instructions alter register values and/or status flags based on the values in registers. They all use an `op2` operand which can be a registers, like `op1` and `dest`, or various other options. See [Flexible Operand 2](https://tomcl.github.io/visual2.github.io/flexop2.html#content) for details.

All DP instructions except Compare have an optional `S` suffix that controls if flags are changed (S) or preserved (no S). Flags are written from the value computed. The value computed is written to dest except in compare instructions.

| Flag | Value written (if changed) |
|:------:|------|
| N | dest < 0 |
| Z | dest = 0 |
| C | Carry out from addition or subtraction <br> Last bit shifted out from shift or rotate |
| V | Signed overflow from addition or subtraction |



#### Arithmetic Instructions


*OpCode dest, op1, op2*

**Examples**

```
SUB R10, R5, R4 ; set R10 equal to R5 - R4
ADD R1, R1, #1  ; add 1 to R1
RSBS R1, R1, #0 ; negate R1 setting NZCV (R1 = 0 - R1)
```


| OpCode | Function | Flags changed <br>if S Suffix|
|------|--------|:-----:|
| **ADD** | dest := op1 + op2 | NZCV
| **SUB** | dest := op1 - op2 | NZCV
| **RSB** | dest := op2 - op1 | NZCV
| **ADC** | dest := op1 + op2 + C | NZCV
| **SBC** | dest := op1 - op2 + C - 1| NZCV
| **RSC** | dest := op2 - op1 + C - 1 | NZCV

#### Logical Instructions


*Opcode dest, op1, op2*


**Examples**

```
ORR R1, R1, #16 ; sets bit 4 in R1
BICS R10, R10, 0x80000; clear bit 19 of R10, setting falgs NZ
```


| OpCode | Function | Flags changed <br> if S Suffix |
|----------|------------|:----:|
| **AND** | dest := op1 Bitwise AND op2| NZ |
| **ORR** | dest := op1 Bitwise OR op2 | NZ
| **EOR** | dest := op1 Bitwise exclusive OR op2 | NZ|
| **BIC** | dest := op1 Bitwise AND NOT op2 | NZ |

#### Move Instructions


*OpCode dest, op2*


**Examples**

```
MVN R1, R1   ; inverts bits in R1
MOVS R10, R3 ; load R10 with copy of value in R3, write flags `NZ`.
```


| OpCode | Function | Flags changed <br> if S suffix |
|----------|------------|:-----:|
| **MOV** | dest := op2| NZ |
| **MVN** | dest := Bitwise invert op2 | NZ |

#### Shift/Rotate instructions


*OpCode dest, op2*


**Examples**

```
LSLS R0, R1, #4     ; same as MOVS R0, R1, LSL #4
ROR  R1, R1, #2     ; same as MOV R1, R1, ROR #2
RRX  R1, R2         ; same as MOV R1, R2, RRX
```

These are shorthand for the corresponding `MOV` with shifted *op2*.

| OpCode | Function |
|----------|------------|
| **LSR** |  Logical Shift Right (shift 0s in) |
| **ASR** | Arithmetic Shift Right (shift sign bit in) | 
| **LSL** | Logical Shift Left | 
| **ROR** | Rotate Right (shift RHS bit in) |
| **RRX** | Rotate Right eXtended - always 1 bit |



#### Compare Instructions

*OpCode op1, op2*


**Examples**

```
CMP R0, R1 ; set `NZCV` flags on subtraction R0 - R1
TEQ R3, R4 ; set `NZ` flags on R3 XOR R4
```


| OpCode | Function | Flags changed |
|----------|------------|:----------------:|
| **CMP** |  set flags on op1 - op2 | NZCV |
| **CMN** | set flags on op1 + op2 | NZCV |
| **TST** | set flags on op1 Bitwise AND op2 | NZ |
| **TEQ** | set flags on op1 Bitwise XOR op2 | NZ |

### Single Register Memory Transfer Instructions

**Examples**

```
LDR R0, [R1,#8] ; load into R0 mem32[R1+8]
STRB R11, [R2,#3]! ; store LS byte R11 in mem8[R2+3], set R2 := R2+3
LDR R0, [R1],#8 ; load into R0 mem32[R1]. then set R1 := R1+8.
LDR R5, =DATA ; load 32 bit immediate value of symbol DATA into R5
```


See [Address Modes](https://tomcl.github.io/visual2.github.io/ea.html#content) for details of **EA** modes.

| OpCode | Instruction | Function | Notes |
|:----|:----------|------------|-------|
| **LDR**|`LDR Rd, EA` | Rd := mem32[EA] | 1
| **LDRB**| `LDRB Rd, EA` | Rd := mem8[EA] | 2
| **STR**| `STR Rs, EA` | mem32[EA] := Rs | 1
| **STRB** |`STRB Rs, EA`| mem8[EA] := Rs | 3
|**LDR =** | `LDR Rd, =Literal` <br> `LDR Rd, =DATA+4` <br> `LDR Rd, =1571`| Rd := Literal | 4


1.  [EA](https://tomcl.github.io/visual2.github.io/ea.html#content) must be divisible by 4.
2. This loads one byte from memory into the LS 8 bits of Rd. <br> The MS 24 bits are zeroed.
3. This writes the LS 8 bits of Rd into the specified memory byte. <br> Other bytes in the same memory word are unaffected.
4. The literal can be any numeric expression of constants and symbols, and does not have a #. Unlike MOV there is no restriction on value. Technically this instruction is a pseudo-instruction, translated by assembler into an `LDR` with EA offset from PC, and a `DCD` that defines the the 32 bit constant. Programmers can use it exactly as `MOV` with literal op2, noting that it is slower than `MOV`.

| **Effective Address EA** | **Address Mode** |
|-----|-------|
| [Rb] | Register |
| [Rb,#N] | Register Offset |
| [Rb,#N]! | Register Pre-increment|
| Rb, #N  | Register Post-increment|
| other  | [Other Modes](https://tomcl.github.io/visual2.github.io/ea.html#content) |

[Details](https://tomcl.github.io/visual2.github.io/ea.html#content)

### Multiple Register Memory Transfer Instructions

**Examples**

```
LDMED R0, {R1-R3,R8,R10} ; pop from ED stack pointer R0 into R1,R2,R3,R8,R10
STMFA R11,{R1,R12}  ; push R1,R12 onto FA stack pointer R11
```

The register list (in `{}`) can contain any distinct set of individual registers or ranges, for 1 - 15 data words transferred. The pointer register must not be in the register list.

The instruction suffix (`FD` here) indicating stack or tranfer type can be any of **FD**,**FA**,**ED**,**EA**,**IA**,**IB**,**DA**,**DB**`. See [suffixes](https://tomcl.github.io/visual2.github.io/suffixes.html) page for details of suffixes and specifcation of which memory addresses are used in the transfer.

A **!** after the first (stack pointer) register means that this is updated as is needed for a stack operation.

|OpCode|| Instruction | Function | Notes |
|---|---|-------------|------------|-------|
|**LDM** | `LDMFD R10!, {R1,R4-R6}` | Load registers R1,R4,R5,R6 <br>Stack Pointer = R10 |update R10|
| | `LDMFD R13, {R1,R10}` | Load registers R1,R10 <br> Stack pointer = R13 |Do not change R13|
|**STM** | `STMFD R8!, {R1,R4-R6}` | Store registers R1,R4,R5,R5 <br> Stack pointer = R8|Update R8|
| | `STMFD R3, {R1,R10,R14}` | Store registers R1,R10,R11,R12,R13 <br> Stack pointer = R3| Do not change R3|

### Pseudo-instructions and Directives

| OpCode | Example | Notes |
|:---:|:----------|-----------|
| **DCD** | `DAT DCD d1, d2,...,dn` | Set next n data words to numeric values <br> given by operands d1,...,dn (no #).<br> Label `DAT` is usual but not compulsory
| **DCB** | `DAT1 DCB d1, d2,...,dn` | Set next n data bytes to numeric values <br> given by operands d1,...,dn (no #).<br> n must be divisible by 4. <br>Bytes are written in little-endian address order. <br> Label `DAT1` is usual but not compulsory.|
| **FILL** | `DAT2 FILL 40` | `40` can be any literal numeric expression (without #). <br> Fill next 40 words of data memory with 0 <br> Label `DAT2` is usual but not compulsory.
| **ADR** | `ADR Ra, LABEL` | Set Ra to `LABEL`. Same as `MOV Ra, #LABEL` but <br> literal values close to PC are allowed.<br> Useful to load data area label values.|
| **EQU** | `LAB1 EQU LABEL + 4`| `EQU` sets its label, in this case `LAB1`, <br>  equal to the given numeric expression.<br> Forward references are allowed. |

See also **LDR =** under [Single Register Memory Transfer](https://github.com/tomcl/visual2.github.io/blob/master/list.md#single-register-memory-transfer-instructions)
