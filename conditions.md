# ARM Condition Codes

ARM condition codes can be added to any valid ARM op-code (but not pseudo-instructions or directives). The condition code comes *after* any instruction suffixes.

An instruction with a condition code will be executed conditionally. Note that if the instruction is not executed there is no state change except for PC increasing to the next instruction. The non-executed instruction takes one cycle, whatever the execution time of the instruction when it is executed.

**Examples**

```
MOVEQ
STRBVC
ADDSGE
LDMFDNE
```

|Code | Condition | Flags |
|-----|-----------|-------|
| EQ | equal | Z set |
| NE | Not equal | Z clear |
| CS/HS | Unsigned &#x2265; <br> *Carry Set* / *High or Same* | C Set |
|CC/LO | Unsigned &lt; <br> *Carry Clear* / *LOw* | C Clear |
|MI | Minus (negative) | N Set|
|PL | Plus (positive or 0) | N clear |
|VS | Signed overflow | V Set|
|VC | No signed overflow | V Clear |
|HI | Unsigned &#8805; <br> *HIgh* | C Set and Z Clear |
|LS| Unsigned &#x2264; <br> *Low or Same* | C clear or Z set |
|GE| Signed &#x2265; | N = V |
|LT| Signed &lt; | N &#x2260; V |
|GT| Signed &gt; | Z clear and N = V |
|LE| Signed &#x2264; | Z set or N &#x2260; V |
|AL| Always (normally omitted) | any |
|NV| Never (do not use this) | none|
