# ARM Data Processing Instructions

These instructions alter register values and/or status flags based on the values in registers.

There are four types of instruction.

<figure>

| Type | Example | Format | Flags <br> Changed | Register <br> Changed |
|------|--------|------------|-------------|----------------|
| Arithmetic | ADD R1, R2, R3 | ADD dest, op1 , op2 | if S suffix | dest |
| Logical | AND R1, R2, R3 | AND dest, op1 , op2 | if S suffix | dest |
| Move | MOV R1, R2 | MOV dest, op2 | If S suffix | dest |
| Compare | CMP R1, R2 | CMP op1, op2 | Always | None |

<figcaption> Data Processing Instriction Types </figcaption>

</figure>

The dest and op1 operands must be registers. _Flexible op2_ is also allowed to be a number (prefixed by #) or a shifted register.

| Op | Example | Format | Notes |
|----------|---------------|
| dest | R5 | Rd (d=0-15) | single register |
| op1 | R3 | Rn  (n=0-15) | single register
| op2 | R1 <br> #101 <br> R1, LSL #1 | Rm (n=0-15)<br> #N (N = op2 integer)<br> (Shifts see table 2)| Flexible op2 can be: <br> register, <br> number, or <br>shifted register |  


