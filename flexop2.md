# Flexible Operand 2

Data processing instruction `op2` has a number of formats. The `SFT` code can be any of `LSL`, `LSR`, `ASR`, `ROR`.

| Op2 format | Examples | Number used in DP <br> instruction |
|------------|---------|---------------------------------|
| Register | `R1` | The value of a register |
| Literal | `#22` | An immediate numeric literal |
| Shift | `R1, LSL #3` | The value of a register <br> shifted by a constant |
| Register-valued Shift | `R1, ASR R2` | The value of a register shifted <br> by the number in another register |
| RRX | `R1, RRX` | `RRX` does not use a literal <br> and always makes a value <br> rotated right by 1 |

## Notes

* Register-valued shifts `Rx, SFT Ry` use the lowest 5 bits of `Ry` to determine the number of bits by which Rx is shifted
* `Rm, RRX`. is a 33 bit rotate right in which Rm(0) -> C -> Rm(31), and all other bits of Rm shift right by 1.

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

