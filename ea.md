# UAL Address Modes

LDR and STR instructions second operand defines an _addressing mode_. 
The addressing mode determines how the _effective address_ of the memory transfer is calculated, and whether the _base register_ is updated
as a side effect of the instruction.

There are four addressing modes shown in the table below, and a special case. The first mode, register addressing, 
is a special case of Offset addressing where OFFSET is 0. Thus there are three main modes.

The base register is Rb, and optionally updated. The OFFSET component can be a 
numeric literal, a register, or a shifted register. 

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
