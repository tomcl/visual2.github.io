# ARM Condition Codes


|Code | Condition | Flags |
|-----|-----------|-------|
| EQ | equal | Z set |
| NE | Not equal | Z clear |
| CS/HS | Unsigned &#x2265 <br> Carry Set / High or Same | C Set |
|CC/LO | Unsigned <&lt> <br> Carry Clear / Low or Same | C Clear |
|MI | Minus (negative) | N Set|
|PL | Plus (positive or 0) | N clear |
|VS | Signed overflow | V Set|
|VC | No signed overflow | V Clear |
|HI | Unsigned &#8805 <br> High | C Set and Z Clear |
|LS| Unsigned &#x2264 <br> Low or Same | C clear or Z set |
|GE| Signed &#x2265 | N = V |
|LT| Signed &lt | N &#x2260 V |
|GT| Signed &gt | Z clear and N = V |
|LE| Signed &#x2264 | Z set and N &#2260 V |
|AL| Always (normally omitted) | any |
|NV| Never (do not use this) | none|
