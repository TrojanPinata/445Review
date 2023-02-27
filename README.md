# ECE 445 Midterm 1 Review By Brian Hill, for Spring 2023

## Lecture 1
### - History - 
- 1939 - First binary digital computer
- - George Stibitz (Bell Labs) - Complex Number Calculator
- - Konrad Zuse (Germany) - First Programmable Calculator (Z2)
- 1943 - First Vacuum-Tube Programmable Logic Calculator
- - Britain - Colossus
- 1946 - First electronic computer put in operation
- - John Mauchy and John Presper Eckert (Univ. Penn) - ENIAC
- 1949 - First Stored-Program computer is built
- - Maurice Wilkes (Britain) - EDSAC
- 1951 - First computer designed for US business
- - Eckert and Mauchly - UNIVAC
- - J. Lyons & Company (Britain) - Lyons Electronic Office
- 1960 - Digital Equipment Corporation introduces the PDP-1
- - PDP-1 Introduced for the science and engineering market
- - PDP-8 is released 1965
- 1975 - First home computer is Marketed to hobbiests
- - Micro Insrumentation Telemetry Systems - Altair 8800
- 1977 - Apple II released
- - Jobs and Wozniak - Apple II
- 1981 - IBM PC
- - International Business Machines - based on Intel 8088
- 1984 - Macintosh introduced
- 2010 - iPad introduced

Moore's law - The number of transistors on an affordable CPU would double every two years. Somewhat disproven.

### - Parts of a processor - 
Datapath
- Register File - contains registers for storing data being used by processor
- ALU - preforms arithmetic and logic operations
- PC - program counter, keeps track of the current instuction being processed

Control Unit - Outside of datapath, controls datapth components

### - Instruction Cycle - 
Three Processes
- Fetch - Instruction is fetched from program memory
- Decode - Instruction is decoded by control unit and control signals are generated
- Execute - Instruction is executed by datapath components

### - Memory Structure - 
Memory is stored a few ways
- Word - 4 bytes
- Half - 2 Bytes
- Byte - 8 Bits

Depending on how close a storage device is to the CPU, the faster it is.

Registers <- Memory <- Storage

----------------------------------------------------------------------------------------------------------------
## Lecture 2
### - Architecture - 
Instruction Set Architecture (ISA) - defineds what a processor can do, it's attributes, and the way it should be programmed

Hardware Implementation - defines how the processor architecture is realized, the technology used, and how data is connected. Results in a family of procesors, each with a different price/preformance specification

Software Development - Compilers, Linkers, Loaders for different processor architectures. High-level programs  written to convert code to Low-level code which can be run natively.

ISA's define:
- Instruction set - what data is being moved, what is done with it, jumps, branches, etc.
- Register set - user accessable registers which are fast and can store operands. On chip, part of datapath
- Addressing modes - determines the position of data in each instruction
- Instruction formats - defines where the operands, operator, destinations, etc. will be in a given instruction
- Data width - result of flexability desired from the processor, as well as size of data wanting to be handled
- Address width - the maximum size of the memory which can be accessed
- Byte-addressable / Word-addressable memory - defines whether the memory is accessed every 4 bytes (word) or by byte
- Endian-ness - determines if MSB or LSB come first (Big Endian has LSB first and Little is opposite)

### - General Processor Models -
Stored Program Computer - computer in which instructions and data are stored in memory.

Von Neumann Architecture - processor model which uses a single bus in order to access instruction and data memory modules. Processor can either read or write, not both (Von Neumann Bottleneck). System requires less complex hardware and uses a single sequential memory.

Harvard Architecture - uses two seperate busses to access instruction memory and data memory. By seperating memory, the processor does not run into bottleneck, but increases complexity and requires seperate memory bandks.

Modified Harvard Architecture - uses a single memory with split cache in order to reduce complexity and reduce bottlenecks. The best of both worlds.

### - RISC vs CISC - 
Reduced Instruction Set Computer (RISC) - processor architecture type which uses simple intructions to define properties and functions. Uniform instruction format. Can be easily pipelined. Simple addressing modes. (See: ARM, RISCV, PowerPC, MIPS, etc.)

Complex Instruction Set Computer (CISC) - complex instructions which can vary in size. Supports high level programming constructs. Uses more clock cycles per instruction. Smaller code size. (See: Intel x86, IBM System/360, PDP-11)

----------------------------------------------------------------------------------------------------------------
## Lecture 3
### - MIPS - 
MIPS stands for Microprocessor without Interlocked Pipeline Stages

MIPS is RISC. MIPS has a many commands - see green card. 

The MIPS ISA defines 32 32-bit registers. There are a few special registers ($zero,  $sp (stack pointer), $fp (frame pointer), $ra (return address register) to name a few). The main ones start with t and s, and are labeled as such depending on how the data is stored in jumps and branches.

MIPS has three main addressing modes. All instructions are 32 bits long, so they need to be flexible. 
- R-type - register operations containing two source operations and one destination register.
- I-type - immediate operations where a register is operated against a value stored in instruction memory which is saved back to a register.
- J-type - contains a address which the program counter is set to directly.

MIPS is bi-endian and can have a maximum of 4GB of memory. Mips is byte addressable.

### - Syntax - 
MIPS assembly has a few parts:
- `label:` - a string with a colon before it signifies a label for a line is written before a instruction
- `addi $t1, $t2, 1` - the instruction to be executed
- `# comment` - comments which are not compiled or executed

R-type operations are in the format `opcode Rd, Rs, Rt`. `Rd` is the destination while `Rs` and `Rt` are the source registers.

I-type operations are slightly different (exception for store commands `sw`, `sh`, and `sb`) being `opcode Rt, Rs, immediate`, where `Rt` is the destination and the immediate value takes the place of a source.

Bitwise shifts are implemented similarly, but the immediate position is filled with the shamt, another part of the instruction, in order to shift the source value. 

The exception with the store commands is that the source and destination values are reversed, as the value in the register is being stored to the immediate value as a address in memory. The opposite of a load or regular immediate operation.

J-type instructions are as simple as can be assumed, containing a opcode and immediate in the form `opecode immediate`.

----------------------------------------------------------------------------------------------------------------
## Lecture 4
### - Machine Language -
MIPS Machine Language is a simple translation of the above assembly code above.

![image of bit breakdown for different addressing modes](https://i.imgur.com/KYWfMBc.png)

### - Branches and Jumps - 
Branches are a way of adding loops and conditional jumps into MIPS. They work by evaluating two sources, and if true, the instruction triggers a jump to another section of instruction. Branches use PC relative addressing, which mean the PC needs to be iterated i blocks of 4 to get where the user desires.

Jumps are similar in regards to esoteric rules. The immediate of a jump command must end in 00 in order to be valid, forcing the user to shift the immeditate left by two.

### - Steps in Generating Machine Code -
Highest level to lowest:

Compiler -> Assembler -> Linker -> Loader

Compiler takes high-level code and converts it into assembly. The assembler takes that code and converts it into machine code. The linker takes libraries and other executables and combines them before handing them to the loader, which directly loads the program into memory.

----------------------------------------------------------------------------------------------------------------
## Lecture 5
### - Other Assembly features -
MIPS assembly code has headers and preloaded data that can be defined at the start and end of the program. These are called directives. For example: 

`.data` can set an immediate in the instruction data to a specific value

`.asciiz` can make a string. Similarly, `.word`, `.byte`, and `.ascii`

`.text` defines instructions the assembler should look for

`.globl main` defines the main function, i.e. the part of the code which the program should start with.


### - Converting C to MIPS -
Addition on multiple items:

`a = b + c + d + e` -- Assuming `$s0 - $s4` = `a - e` 

`add $s0, $s1, $s2`

`add $s0, $s0, $s3`

`add $s0, $s0, $s4`

If-then-else statements:

`if (a == b) { c = 1; }`

`else { c = 0; }`

becomes

`beq $s5, $s6, THEN`

`addi $s7, $zero, 0 # ELSE condition`

`j Done`

`THEN: addi $s7, $zero, 1 # THEN condition`

`Done: … # next instruction`

----------------------------------------------------------------------------------------------------------------
## Lecture 6
### - Implementations of MIPS: Single-Cycle -
Simplest form of MIPS processor. One clock cycle for each instruction. Clock speed depends on slowest instruction possible.

![Single-Cycle diagram](https://i.imgur.com/VbpdkXr.png)

### - Implementations of MIPS: Multi-Cycle -
Splits up instructions into multiple stages. One clock cycle for each stage. Clock speed is determined based on longest stage. Different instructions require different number of stages. Datapath is more efficient than Single-cycle.

![Multi-cycle diagram](https://i.imgur.com/XJcYk4v.png)

### - Implementations of MIPS: Pipelined -
Similar to Multi-cycle, but utilizes busses and registers to use a higher percentage of the processor that would otherwise be unutilized. All instructions require same amount of time, same as longest stage, but overall time is reduced due to being able to run multiple stages simultaneously.

![Pipelined implementation diagram](https://i.imgur.com/ZCOIIh3.png)

**Understanding the datapath is extremely important, go over the slides before the exam to make sure you get whats going on.**

----------------------------------------------------------------------------------------------------------------
## Lecture 7
### - RTL -
Register Transfer Language (RTL) is a symbolic language used to describe what is going on inside a computer. Made for humans rather than computers. Describes what is going on on each clock cycle.

Registers are denoted as `Rn`, where `n` is a number describing a register. `<-` indicates a transfer of information from one register to another and commas denote a transfer occuring during the same clock cycle. The operations RTL describes are called micro-operations.

Here is a example of describing a instruction fetch in RTL:

![inst. fetch in RTL](https://i.imgur.com/wLvIpoh.png)

Here is a example of lw (load word):

![lw in RTL](https://i.imgur.com/gEXJsXM.png)

----------------------------------------------------------------------------------------------------------------
## Lecture 8

Note: this is my least confident section to explain, read the slides for a better example.

### - Function Calls - 
In C and many other programming languages take in parameters and are unable to modify values outside of the function (unless global). Similarly, the function in assembly takes in parameters and can only work with those in the registers. Such, the values already there are saved to memory. In a way, the caller function is preserved and reloaded after the function is executed, getting the returned value from a special register called the return address register. The preserved registers include the `$sn` (saved) registers, the `$sp` (stack pointer), and `$ra` (return address register). 

### - The Stack -
Last-in First-out data structure which is located in main memory. Used to store arguments and returned data, as well as all preserved information. The two main ppinters are:
- Frame Pointer (`$fp`) - Points to the last item pushed to the stack. At the top of the stack. 
- Stack Pointer (`$sp`) - Points to bottom where function was initially called. Remains fixed until return is called. 

### - Activation Record -
The Activation Record or stack fram is the part of the stack currently being used by a executing function. Everytime a function is called, the activation record changed. Nested function calls take advantage of this.

----------------------------------------------------------------------------------------------------------------
## Lecture 9
### - Microcode -
Microcode is the concept of writing the lowest level of firmware possible for a processor in order to allow changes and modifications of the control unit without permenently changing the hardware. The benefit of this is that bugs can be changed in development and patches can be deployed to improve security. This method replaces old school ROMs and PLAs which are permenent implementations of code.

Bonus multi-cycle FSM

![FSM for Multi-cycle implmentation](https://i.imgur.com/KafXFmc.png)
