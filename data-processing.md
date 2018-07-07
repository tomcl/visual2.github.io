# ARM Data Processing Instructions

These instructions alter register values and/or status flags based on the values in registers.
There are four types of instruction.

## Instruction Types



| Type | Example | Format | Flags <br> Changed | Register <br> Changed |
|------|--------|------------|-------------|----------------|
| [Arithmetic](dp-list.html#arithmetic) | ADD R1, R2, R3 | ADD dest, op1 , op2 | if S suffix | dest |
| [Logical](dp-list.html#logical) | AND R1, R2, R3 | AND dest, op1 , op2 | if S suffix | dest |
| [Move](dp-list.html#move) | MOV R1, R2 | MOV dest, op2 | If S suffix | dest |
| [Compare](dp-list.html#compare) | CMP R1, R2 | CMP op1, op2 | Always | None |


## Operand Formats

The dest and op1 operands must always be registers. _Flexible op2_ is also allowed to be a number (prefixed by #) or a shifted register.

| Op | Example | Format | Notes |
|----------|---------------|
| dest | R5 | Rd (d=0-15) | single register |
| op1 | R3 | Rn  (n=0-15) | single register
| op2 | R1 <br> #101 <br> R1, LSL #1 | Rm (n=0-15)<br> #N (N = op2 integer)<br> ([Shifts](flexop2.html))| [Flexible op2](flexop2.html) |  


