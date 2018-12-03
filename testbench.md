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
  - `IN R0 is 221` ; regsiters can be given assigned values on input
  - `IN R4 ptr 1,10,3,100` ; A register will be set up pointing to the allocated data in memory
  - `OUT R5 is 22` ; Outputs can be registers checked for value, 
  - `OUT R5 ptr 100,200,300` ; the memory from address given by *input value* of R5 must be filled as stated on output.

- Additional tests:
  - `PERSISTENTREGS R10,R11`  ; check that given registers are unchanged by program (normally the case for persistent registers)
  - `STACKPROTECT`  ; check that locations on stack above those used by program are not overwritten
  - `DATAAREA 0x3000` ; specify where IN and OUT memory data is put. If not specified the default will work, each IN and OUT with memory data is assigned a unique area in memory
  
  

## Example 1

```
##TESTBENCH

#TEST 1
IN R0 is 1
IN R1 is 2
OUT R2 is 3
IN R3 ptr 1,2,3,7,11
IN R4 ptr 6,10,88,100,900

#TEST 2
IN R0 is 1
IN R1 is 2
OUT R2 is 3
IN R3 ptr 1,2,3,7,11
IN R4 ptr 6,10,88,100,900
```

- Load this code into a VisUAL2 buffer. Create another buffer (just one) which is empty.
- run the testbench as follows
  - Run button on testbench buffer
  - Run button -> testbench on (empty) code buffer
- Note the errors highlighted in red in the testbench file
- Add `ADD R2,R1,R0` to the code buffer
- Rerun the testbench
- Both tests pass (green annotations in testbench file)
- Do: Run Menu -> Step into test -> Test 1
  - This starts the code with the initial data from the testbench
  - Check the contents of registers and memory, see how this relates to the testbench `IN` data definitions




