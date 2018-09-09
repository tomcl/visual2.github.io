# ARM Condition Codes


|Code | Condition | Flags |
| EQ | equal | Z set |
| NE | Not equal | Z clear |
| CS/HS | Unsigned &geq <br> Carry Set / High or Same | C Set |
|CC/LO | Unsigned < <br> Carry Clear / Low or Same | C Clear |
|MI | Minus (negative) | N Set|
|PL | Plus (positive or 0) | N clear |
|VS | Signed overflow | V Set|
|VC | No signed overflow | V Clear |
|HI | Unsigned > <br> High | C Set and Z Clear |
|LS| Unsigned &leq <br> Low or Same | C clear or Z set |
|GE| Signed &geq | N equals V |
|LT| Signed < | N is not equal V |
|GT| Signed > | Z clear and N equals V |
|LE| Signed &leq | Z set and N not equal to V |
|AL| Always (normally omitted) | any |
|NV| Never (do not use this) | none|
