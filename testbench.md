# VisUAL2 Testbenches

A file loaded into VisUAL2 is considered a testbench if it starts with the line:

```
##TESTBENCH
```

Subsequent lines make up a sequence of 1 or more **Tests**. Each Test specifies *inputs* and *outputs* used in testing an assembler program.

```
#TEST 1
<lines of Test 1>
#TEST 2
<lines of test 2>
```

- I/O tests
  - IN R0 is 221 ; regsiters can be given assigned values on input
  - IN R4 ptr 1,10,3,100 ; A register will be set up pointing to the allocated data in memory
  - OUT R5 is 22 ; Outputs can be registers checked for value, 
  - OUT R5 ptr 100,200,300 ; the memory from address given by *input value* of R5 must be filled as stated on output.

- Additional tests:
  - PERSISTENTREGS Ra,Rb  ; check that given registers are unchanged by program (normally the case for persistent registers)
  - STACKPROTECT  ; check that locations on stack above those used by program are not overwritten
  - DATAAREA 0x3000 ; specify where IN and OUT memory data is put. If not specified the default will work, each IN and OUT with memory data
  is assigned a unique area in memory
