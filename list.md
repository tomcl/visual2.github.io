## UAL Instructions


### Data Processing Instructions

These instructions alter register values and/or status flags based on the values in registers. They all use an `op2` operand which can be a registers, like `op1` and `dest`, or various other options. See [Flexible Operand 2](flexop2.html) for details.

All DP instructions except Compare have an optional `S` suffix that controls if flags are changed (S) or preserved (no S). Flags are written from the value computed. The value computed is written to dest except in compare instructions.

| Flag | Value written (if changed) |
|:------:|------|
| N | dest < 0 |
| Z | dest = 0 |
| C | Carry out from addition or subtraction <br> Last bit shifted out from shift or rotate |
| V | Signed overflow from addition or subtraction |



#### Arithmetic Instructions

```
OpCode dest, op1, op2
```
Examples:
```
SUB R10, R5, R4 ; set R10 equal to R5 + R4
ADD R1, R1, #1  ; add 1 to R1
RSBS R1, R1, #0 ; negate R1 setting NZCV
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

```
Opcode dest, op1, op2
```
Example
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

```
OpCode dest, op2
```
Examples
```
MVN R1, R1   ; inverts bits in R1
MOVS R10, R3 ; load R10 with copy of value in R3, write flags `NZ`.
```


| OpCode | Function | Flags changed <br> if S suffix |
|----------|------------|:-----:|
| **MOV** | dest := op2| NZ |
| **MVN** | dest := Bitwise invert op2 | NZ |


#### Compare Instructions

```
OpCode op1, op2
```

Examples
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

| EA | Address Mode |
|-----|-------|
| [Rb] | Register |
| [Rb,#N] | Register |
| [Rb,#N]! | Register pre-increment|
| Rb, #N  | register post-increment|
| other  | [Other modes](https://tomcl.github.io/visual2.github.io/ea.html#content) |


See [Address Modes](https://tomcl.github.io/visual2.github.io/ea.html#content) for more details and additional modes.

| Instruction | Function | Notes |
|:----------|------------|-------|
| **LDR Rd, EA** | Rd := mem32[EA] | [EA](https://tomcl.github.io/visual2.github.io/ea.html#content) is divisible by 4
| **LDRB Rd, EA** | Rd := mem8[EA] | 
| **STR Rs, EA** | mem32[EA] := Rs | [EA](https://tomcl.github.io/visual2.github.io/ea.html#content) is divisible by 4
| **STRB Rs, EA**| mem8[EA] := Rs


### Multiple Register Memory Transfer Instructions

The register list (in `{}`) can contain any distinct set of individual registers or ranges, for 1 - 15 data words transferred. The pointer register must not be in the register list.

The instruction suffix (`FD` here) indicating stack or tranfer type can be any of `FD,FA,ED,EA,IA,IB,DA,DB`. See [suffixes](https://tomcl.github.io/visual2.github.io/suffixes.html) page for details of suffixes and specifcation of which memory addresses are used in the transfer.

| Instruction | Function | Notes |
|-------------|------------|-------|
| **LDMFD R10!, {R1,R4-R6}** | Load registers R1,R4,R5,R6 <br> from FD stack with stack pointer R10 |update R10|
| **LDMFD R13, {R1,R10}** | Load registers R1,R10 <br> from FD stack with stack pointer R13 |Do not change R13|
| **STMFD R8!, {R1,R4-R6}** | Store registers R1,R4,R5,R5 <br> to FD stack with stack pointer R8|Update R8|
| **STMFD R3, {R1,R10,R14}** | Store registers R1,R10,R11,R12,R13 <br> to FD stack with stack pointer R3| Do not change R3|

### Pseudo-instructions and Directives

| OpCode | Example | Notes |
|:---:|:----------|-----------|
| **DCD** | **DAT DCD d1, d2,...,dn** | Set next n data words to numeric values <br> given by operands d1,...,dn (no #).<br> Label `DAT` is usual but not compulsory
| **DCB** | **DAT1 DCB d1, d2,...,dn** | Set next n data bytes to numeric values <br> given by operands d1,...,dn (no #).<br> n must be divisible by 4. <br>Bytes are written in little-endian address order. <br> Label `DAT1` is usual but not compulsory.|
| **FILL** | **DAT2 FILL EXPR** | EXPR is a literal expression (without #). <br> Fill next EXPR words of data memory with 0 <br> Label `DAT2` is usual but not compulsory.
| **ADR** | **ADR Ra, LABEL** | Set Ra to `LABEL`. Same as `MOV Ra, #LABEL` but <br> literal values close to PC are allowed.<br> Useful to load data area label values.|
| **EQU** | **LAB1 EQU LABEL + 4**| `EQU` sets its label, in this case `LAB1`, <br>  equal to the given constant expression.<br> Forward references are allowed. |
