# ARM DP Instruction List

These instructions alter register values and/or status flags based on the values in registers.
There are four types of instruction.


### Arithmetic

```
OpCode dest, op1, op2
```


| OpCode | Function | Flags changed if S |
|------|--------|
| ADD | dest := op1 + op2 | NZCV
| SUB | dest := op1 - op2 | NZCV
| RSB | dest := op2 - op1 | NZCV
| ADC | dest := op1 + op2 + C | NZCV
| SBC | dest := op1 - op2 + C - 1| NZCV
| RSC | dest := op2 - op1 + C - 1 | NZCV

### Logical

```
Opcode dest, op1, op2
```


| OpCode | Function | Flags changed if S |
|----------|------------|
| AND | dest := op1 Bitwise AND op2| NZ |
| ORR | dest := op1 Bitwise OR op2 | NZ
| EOR | dest := op1 Bitwise exclusive OR op2 | NZ|
| BIC | dest := op1 Bitwise AND NOT op2 | NZ |

### Move

```
OpCode dest, op2
```


| OpCode | Function | Flags changed if S
|----------|------------|
| MOV | dest := op2| NZ |
| MVN | dest := Bitwise invert op2 | NZ |


### Compare

```
OpCode op1, op2
```


| OpCode | Function | Flags changed |
|----------|------------|
| CMP |  set flags on op1 - op2| NZCV |
| TST | set flags on op1 Bitwise AND op2 | NZ |


