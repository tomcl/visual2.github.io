# Flexible Op2
 
- MOV Rd, op2 ; op2 in two operand MOV instruction
- ADD Rd, Rs, op2 ; op2 in 3 operand instruction
- CMP Rs, op2 ; op2 in 2 operand copare instruction

Data processing instruction `op2` has a number of formats. 

| Op2 format | Examples | Number used in DP <br> instruction |
|------------|---------|---------------------------------|
| Register | `MOV R0, R1` | The value of a register |
| Literal | `ADD R0, R1, #22` | An immediate numeric literal |
| Shift | `MVNS R0, R1, LSL #3` | The value of a register <br> shifted by a constant |
| Register-valued Shift | `MOV R0, R1, ASR R2` | The value of a register shifted <br> by the number in another register |
| RRX | `MOV R0, R1, RRX` | `RRX` does not use a literal <br> and always makes a value <br> rotated right by 1 |

## Notes

* `MOV` instructions with shift/rotate can use the shift as a short-form alias:
  - `MOV R0, R1, ASR #4` -> `ASR R0, R1, #4`
  - `MOV R3, R3, RRX` -> `RRX R3,R3`
* Register-valued shifts `Rx, LSL Ry`. The `LSL` code can be any of `LSL`, `LSR`, `ASR`, `ROR`. In all cases the LS 5 bits of `Ry` determine the number of bits by which Rx is shifted or rotated.
* `Rm, RRX`. is a 33 bit rotate right in which Rm(0) -> C -> Rm(31), and all other bits of Rm shift right by 1.
* Note that for all shifted op2 `MOV R0, R1, LSL #3` the shifted register `R1` does not change. Its value, shifted, is used as the op2 in the instruction operation. This value is not written back into R1.

## Numeric Literals
* Allowed immediate literal values:
  * Any positive value <= 255. e.g. #22
  * For most instructions any negative value >= -255. E.g. #-22.
  * Literals may be written in binary #0b1011 or hexadecimal #0x6a. 
  * Combinations of constant symbols and operators: `#DATA1+16`. `#17-0x40`

## Carry written by flexible op2

When carry is not written by arithmetic, and `S` suffix is present, `C` (but not `V`) may be written by op2 in the follwing cases:
* For `MOVS,MVNS,ADDS,ORRS,EORS,BICS`, with shifts or rotates (including `RRX`) the bit last shifted or rotated out sets C. For `ADDS` etc the arithmetic Carry out is defined to set C and takes priority over this.
* A limited number of literal values **larger than 8 bits** are formed by rotating an 8 bit number N:  0 < N < 256 by `2r` bits where 1 <= r <= 15. In this case for `MOVS,MVNS,ADDS,ORRS,EORS,BICS` instructions `C` is set by the last bit rotated out from the literal rotation. Note in particular that therefore all numbers with single bits set to 1 are valid literals.

