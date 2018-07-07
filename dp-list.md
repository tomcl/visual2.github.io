# ARM DP Instruction List

These instructions alter register values and/or status flags based on the values in registers.
There are four types of instruction.


### Arithmetic

| OpCode | Function |
|------|--------|
| ADD | dest := op1 + op2 |
| SUB | dest := op1 - op2 |
| RSB | dest := op2 - op1 |
| ADC | dest := op1 + op2 + C |
| SBC | dest := op1 - op2 + C - 1|
| RSC | dest := op2 - op1 + C - 1 |

### Logical

| OpCode | Function |
|----------|------------|
| AND | dest := op1 Bitwise AND op2|
| ORR | dest := op1 Bitwise OR op2 |
| EOR | dest := op1 Bitwise exclusive OR op2 |
| BIC | dest := op1 Bitwise AND NOT op2 |





