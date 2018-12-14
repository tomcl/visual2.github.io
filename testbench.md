# VisUAL2 Testbenches

To get started quickly go through the [Tutorial](#Tutorial).

Visual2 allows predefined tests written in a *testbench* file to be run on an assembler file. Each test will

- Set up input data
- Run (or branch to a specified subroutine in) the tested program
- Check the results against written answers

There are two ways to run a testbench `tb.s` on an assembly file `myprog.s`.

1. Load `tb.s` and `myprog.s` with *no other* `.s` file. Run *Test -> Run All Tests*
2. Load `tb.s` and `myprog.s` with *no other* `.s` file. View the tab containing `tb.s`. Click `Run`.

When a testbench is run it will go through its tests and mark each test (with red or green lines in the testbench tab) to say whether it has passed or failed. If a test fails no additional tests will be checked.

## Debugging Tested Code

- The initial data (memory and registers) from a single testbench file Test can be used when single-stepping through a program with   *Test-> Step into test->(select test)*. The program will start running with the Test initial data and stop on its first instruction.
- Note that this is also a convenient way to see exactly what data a testbench will set up

## Writing Testbenches

### Syntax

- Case sensitivity: all testbench keywords are case insensitive. Subroutine names are case senitive since in ARM UAL labels are all case sensitive.
- Blank lines anywhere are ignored
- Initial space on each line is ignored


### Header

A `.s` file loaded into VisUAL2 is considered a testbench if it starts with the line:

```
##TESTBENCH  other characters form comment
```

### Tests

Subsequent lines make up a sequence of 1 or more **Tests**. Each Test consists of lines that specify *inputs* and *outputs* used in testing an assembler program, and other test parameters, and starts with a single-line Test header prefixed by #.

Within the lines of a test each line is parsed separately. Order of lines does not matter.

```
#TEST 1
<lines of Test 1>
#TEST 2
<lines of test 2>
```

- I/O tests
  - `IN R0 is 221` ; Registers can be given assigned values on input
  - `IN R4 ptr 1,10,3,100` ; A register will be set up pointing to the allocated data in memory
  - `OUT R5 is 22` ; Outputs can be registers checked for value, 
  - `OUT R5 ptr 100,200,300` ; The memory from address given by *input value* of R5 must be filled as stated on output.

- Additional tests:
  - `PERSISTENTREGS R10,R11`  ; Check that given registers are unchanged by program (normally the case for persistent registers)
  - `STACKPROTECT`  ; Check that locations on stack above those used by program are not overwritten
  - `DATAAREA 0x3000` ; Specify where IN and OUT memory data is put. If not specified the default will work, each IN and OUT with memory data is assigned a unique area in memory
  - `RANDOMISEINITVALS` ; Make all registers not otherwise set have random values
  - `BRANCHTOSUB SUBNAME`; Enter program from testbench with BL SUBNAME. Expect a subroutine return to terminate program code. This allows subroutines submitted standalone to be tested.
  
  

## Tutorial

```
##TESTBENCH

#TEST 1
IN R0 is 1
IN R1 is 2
OUT R2 is 3
IN R3 ptr 1,2,3,7,11
IN R4 ptr 6,10,88,100,900

#TEST 2
IN R0 is 10
IN R1 is 200
OUT R2 is 210
IN R3 ptr 1,2,3,7,11
IN R4 ptr 6,10,88,100,900
```

- Load this code into a VisUAL2 buffer. Create another buffer (just one) which is empty.
- Run the testbench as follows
  - Run button on testbench buffer
  - Test menu -> testbench on (empty) code buffer
- Note the errors highlighted in red in the testbench file
- Add `ADD R2,R1,R0` to the code buffer
- Rerun the testbench
- Both tests pass (green annotations in testbench file)
- Do: Test Menu -> Step into test -> Test 1
  - This starts the code with the initial data from the testbench
  - Check the contents of registers and memory, see how this relates to the testbench `IN` data definitions
- Run `test-> Step into test-> Test 1`
 - View the data setup in registers and memory to see how the `ptr` inputs work.




