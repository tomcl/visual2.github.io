# UAL Address Modes

LDR, LDRB, STR, STRB instructions second operand defines an _addressing mode_. 
The addressing mode determines how the _effective address_ of the memory transfer is calculated, and whether the _base register_ is updated as a side effect of the instruction.

ARM has three addressing modes: _offset_, with optional _pre-_ or _post-_ indexing. The first line syntax, _register addressing_, 
is a special case of Offset addressing where OFFSET is 0.

The base register is Rb, and optionally updated. The OFFSET component, added to the base register value to make the effective address,  can be a  numeric literal, a register, or a shifted register. 

| Mode | Second Operand Format | Effective <br> Address | Update|
|------|-------------------|-----------------------|--------|
| Register | [Rb] | Rb | n/a
| Offset | [Rb,OFFSET] | Rb + N | no
| Pre-indexed offset | [Rb,OFFSET]! | Rb + OFFSET | Rb + OFFSET
| Post-indexed offset | [Rb], OFFSET | Rb + N | 

| OFFSET | Format | Notes |
|--------|-------|--------|
| Numeric | #N | N is a positive or negative literal |
| Register | Rx |   |
| Scaled register | Rx, LSL #S | ASR, LSR also allowed. 0 < S < 32 is the number of bits Rx is shifted to make OFFSET |

## Examples

```
; In all these examples data is transferred between memory and R0
LDR  R0, [R1]         ; register addressing, the effective address is the value stored in R1
STR  R0, [R1, #-100]  ; a numeric offset can be used: EA is R1 - 100
LDRB R0, [R1, #LAB]   ; numeric offsets can be assembler symbols
STR  R0, [R1,#0xa0]!  ; pre-indexed numeric offset
LDR  R0, [R1, R2]!    ; pre-indexed register offset
LDR  R0, [R1, R2, LSL #2]! ; pre-indexed scaled register offset
STRB R0, [R1], R2, LSL #2  ; post-indexed scaled register offset
```
