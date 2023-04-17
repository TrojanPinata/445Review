# ECE 445 Review By Brian Hill, for Spring 2023

## Midterm 1

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
- 1975 - First home computer is Marketed to hobbyists
- - Micro Instrumentation Telemetry Systems - Altair 8800
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
- PC - program counter, keeps track of the current instruction being processed

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
Instruction Set Architecture (ISA) - defines what a processor can do, it's attributes, and the way it should be programmed

Hardware Implementation - defines how the processor architecture is realized, the technology used, and how data is connected. Results in a family of processors, each with a different price/performance specification

Software Development - Compilers, Linkers, Loaders for different processor architectures. High-level programs  written to convert code to Low-level code which can be run natively.

ISA's define:
- Instruction set - what data is being moved, what is done with it, jumps, branches, etc.
- Register set - user accessible registers which are fast and can store operands. On chip, part of datapath
- Addressing modes - determines the position of data in each instruction
- Instruction formats - defines where the operands, operator, destinations, etc. will be in a given instruction
- Data width - result of flexibility desired from the processor, as well as size of data wanting to be handled
- Address width - the maximum size of the memory which can be accessed
- Byte-addressable / Word-addressable memory - defines whether the memory is accessed every 4 bytes (word) or by byte
- Endian-ness - determines if MSB or LSB come first (Big Endian has LSB first and Little is opposite)

### - General Processor Models -
Stored Program Computer - a computer in which instructions and data are stored in memory.

Von Neumann Architecture - processor model which uses a single bus in order to access instruction and data memory modules. Processor can either read or write, not both (Von Neumann Bottleneck). System requires less complex hardware and uses a single sequential memory.

Harvard Architecture - uses two separate buses to access instruction memory and data memory. By separating memory, the processor does not run into a bottleneck, but increases complexity and requires separate memory banks.

Modified Harvard Architecture - uses a single memory with split cache in order to reduce complexity and reduce bottlenecks. The best of both worlds.

### - RISC vs CISC - 
Reduced Instruction Set Computer (RISC) - processor architecture type which uses simple instructions to define properties and functions. Uniform instruction format. Can be easily pipelined. Simple addressing modes. (See: ARM, RISCV, PowerPC, MIPS, etc.)

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

J-type instructions are as simple as can be assumed, containing a opcode and immediate in the form `opcode immediate`.

----------------------------------------------------------------------------------------------------------------
## Lecture 4
### - Machine Language -
MIPS Machine Language is a simple translation of the above assembly code above.

![image of bit breakdown for different addressing modes](https://i.imgur.com/KYWfMBc.png)

### - Branches and Jumps - 
Branches are a way of adding loops and conditional jumps into MIPS. They work by evaluating two sources, and if true, the instruction triggers a jump to another section of instruction. Branches use PC relative addressing, which mean the PC needs to be iterated i blocks of 4 to get where the user desires.

Jumps are similar in regards to esoteric rules. The immediate of a jump command must end in 00 in order to be valid, forcing the user to shift the immediate left by two.

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
Splits up instructions into multiple stages. One clock cycle for each stage. Clock speed is determined based on the longest stage. Different instructions require different number of stages. Datapath is more efficient than Single-cycle.

![Multi-cycle diagram](https://i.imgur.com/XJcYk4v.png)

### - Implementations of MIPS: Pipelined -
Similar to Multi-cycle, but utilizes buses and registers to use a higher percentage of the processor that would otherwise be unutilized. All instructions require the same amount of time, same as longest stage, but overall time is reduced due to being able to run multiple stages simultaneously.

![Pipelined implementation diagram](https://i.imgur.com/ZCOIIh3.png)

**Understanding the datapath is extremely important, go over the slides before the exam to make sure you get what’s going on.**

----------------------------------------------------------------------------------------------------------------
## Lecture 7
### - RTL -
Register Transfer Language (RTL) is a symbolic language used to describe what is going on inside a computer. Made for humans rather than computers. Describes what is going on each clock cycle.

Registers are denoted as `Rn`, where `n` is a number describing a register. `<-` indicates a transfer of information from one register to another and commas denote a transfer occurring during the same clock cycle. The operations RTL describes are called micro-operations.

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
Last-in First-out data structure which is located in main memory. Used to store arguments and returned data, as well as all preserved information. The two main pointers are:
- Frame Pointer (`$fp`) - Points to the last item pushed to the stack. At the top of the stack. 
- Stack Pointer (`$sp`) - Points to bottom where function was initially called. Remains fixed until return is called. 

### - Activation Record -
The Activation Record or stack frame is the part of the stack currently being used by a executing function. Every time a function is called, the activation record changed. Nested function calls take advantage of this.

----------------------------------------------------------------------------------------------------------------
## Lecture 9
### - Microcode -
Microcode is the concept of writing the lowest level of firmware possible for a processor in order to allow changes and modifications of the control unit without permanently changing the hardware. The benefit of this is that bugs can be changed in development and patches can be deployed to improve security. This method replaces old school ROMs and PLAs which are permanent implementations of code.

Bonus multi-cycle FSM

![FSM for Multi-cycle implementation](https://i.imgur.com/KafXFmc.png)

----------------------------------------------------------------------------------------------------------------

## Midterm 2

## Lecture 10
### - Processor Performance -
Abbreviations
- ET - Execution Time
- CC - Clock Cycles
- CPI - Clock Cycles Per Instruction
- PERF - Performance (only for this document)
- Tclk - Clock period
- Fclk - Clock frequency
- IC - Instruction Count


There are two metrics in which processor performance can be compared. 
- Time Metric - The amount of time it takes to execute a program (ET)
- Space Metric - The amount of memory required by a program to run.

Time metrics are based around latency, execution time and throughput. Instruction latency is affected by the speed at which a processor can execute a instruction. This is affected by faster processors, though remains the same with more of them. This is different to throughput, where more processors is more throughput and faster processors also means more throughput. Processor performance is all of these things together after taking into account the properties of the instructions and processing type.

Examples: PyTorch is a very heavy program used for basic AI programs which can (depending on what it's used for) use a ton of memory. For example, let’s say we have a program that does something to an image, say upscaling. The more memory it uses, the more of the image it can process at a given time, but it also uses up all of the memory. If it did not, the execution time would be much longer as it would have to break up the image into chunks to process, thus reducing memory load. In this case, the time and space metrics are linearly correlated (sort of). As space requirements increase, processing time increases. This is not always the case but shows the balance between the two in this situation.

### - Math -
PERF = 1/ET
PERFx > PERFy when ETx < ETy

Ratio of performance between processors x and y:

PERFx / PERFy = ETy / ETx

If ratio > 1, x > y by factor of ratio. Otherwise, x < y by factor of 1 / ratio

This same equation can be used with the same processor before and after modifications (its a different processor after all see: [Ship of Theseus](https://en.wikipedia.org/wiki/Ship_of_Theseus))

CCtot (total number of clock cycles to perform a operation) = CPIavg * IC

Use algebra to break back down to components. CPIavg can be derived from multiplying frequency of operation by CPI for instruction and summed with other instruction classes as well.

ET = CCtot * Tclk

MIPS (millions of instructions per second) = IC / ET*10^6 = Clock Rate / CPIavg*10^6

### - Improving Processor Performance -
Three core ways to improve performance
- Reduce instruction count
- Decrease average CPI
- Increase clock frequency

ET = IC * CPIavg * Tclk <= Memorize this

### - Amdahl's Law -
What extent does a given modification to a processor affect it's performance?

ETafter = (ETaffected/Nimporvement) + ETunaffected

Amdahl's Law states that modifications should be made to the parts of the processor that have the greatest impact on the execution time of a program.

### - Benchmarking -
This section is dumb, all you need to know is that there are benchmark programs to figure out how good processors do certain tasks. Usually, they are the kernel to a real program to accurately judge and compare.

The big suite we talked about is SPECint2006. uses a bunch of metrics like total execution time, arithmetic mean, weighted arithmetic mean, geometric mean, and harmonic mean. Not that you need to know much of this tbh.

## Lecture 11
### - Pipelined Processing -

Here is the best way I can describe this. This:

![Non-Pipelined](https://i.imgur.com/0mzdZFZ.png)

is way slower than this:

![Pipelined](https://i.imgur.com/FifCyF7.png)

The main idea is that it is way faster to utilize each step of the datapath for a operation instead of letting each one pass through one after another. It increases efficiency but adds a whole host of other problems. The main improvement is the dramatic increase in throughput. This is accomplished by adding pipeline registers into the datapath which store the output of the previous stage and output it every clock cycle. This has the advantage of allowing the processor to essentially be a multicycle in terms of how it processes instructions, but a single cycle in frequency of getting output results.

MIPS usually breaks down this pipeline into 5 stages.

![Stages](https://i.imgur.com/QiCH0HX.png)

A more detailed breakdown looks like this:

![Details](https://i.imgur.com/GqyZ2o6.png)

To reference the chart from earlier:

![Breakdown](https://i.imgur.com/nN4hW8f.png)

The number of clock cycles it takes should always be the number of instructions + (number of stages - 1)

## Lecture 12/13/14
### - Pipeline Hazards -
There are three main kinds of pipeline hazards: 
- RAW - read after write
- WAR - write after read
- WAW - write after write

And there are three types of pipeline hazards:
- Resource (Structural) Hazards
- Data Hazards
- Control (Branch) Hazards

RAW hazards can be called data dependencies while all three can be data hazards. A option for preventing a hazard (more like repairing) would be to stall the pipeline in the stages before the dependency. NOP (no operations) are inserted by a special component called the **Hazard Detection Unit**. Stalling, however, is not the best solution as it reduces efficiency and wastes clock cycles. Forwarding (which we will talk about in a minute) is a much better option.

Resource hazards (not to be confused with data hazards) occur when two instructions compete for the same internal resource. This is usually because of multiple instructions being operated on and trying to access memory, a alu, etc.

This is one of the units I think it is better to read the slides for as the examples are extremely important to understand.

### - Forwarding - 
The objective of forwarding is to minimize the number of clock cycles that the processor is stalled. The way this happens is to not wait for the write-back stage of the pipeline and **forward** the information directly to the stage where it is needed. Sometimes this is not possible, like in load instructions, and stalling is necessary. This also needs a special component called the **Forwarding Unit**.

Again, please read through the slides for examples as they are really good for this unit. More than half of the slides are examples.

### - Branch Prediction - 
Remember those control hazards that were mentioned earlier? Those are due to problems that can occur with beq and bne instructions. Let's say there is branch instruction making a loop. There is a chance the loop will end on each cycle, and because of that there is a chance it will happen or not happen. This is determined in the MEM stage of the pipeline, so it is more efficient if the processor loads the previous three stages with the right instructions. This is the premise of branch prediction and reducing control hazards.

Branch prediction comes in a few different flavors:
- Static Branch prediction
   - guesses off of most common outcome
- Dynamic Branch Prediction
   - 1-bit predictions: Stores the result of the last branch and chooses that 
   - 2-bit predictions: Determines the likelihood of the next branch based off of the previous two branches and makes a judgement off of that

## Lecture 15
### - Computer Memory -
Memory Systems have a primary system bus with three main lines:
- The Address Bus
- The Data Bus
- The Control Bus

To be clear, memory is NOT part of the datapath.

This section is so difficult to get across via text, but I will try.

Memory systems are composed of memory chips. Each chip has specifications in the format of NK x b. The capacity of a chip is the multiplication of these together. If a chip is 4K x 8, the capacity is 32 Kbits. There is 8 bits per block a address points to, which designates N. The number of bits a address needs to represent all locations is usually log2(N) - 1.

To get more memory out of these chips, a bank system is established. Two chips placed vertically form a bank. These chips are connected in series to extend the depth of the system. Chips connected in parallel and horizontally make the system wider, able to store more bits per address.

![Memory System](https://i.imgur.com/drlCpKC.png)

To make the bank system work, a decoder is needed to switch the bank the address is sent to. This results in a few bits being designated specifically for the bank. This is why bank maps exist. They demonstrate how the banks are split with the addresses.

### - Memory Characteristics -
There are five key characteristics memory can have:
- Volatility - Whether or not the memory module requires power to maintain stored information
   - RAM usually is volatile while ROM, Flash, physical media is non-volatile
- Accessibility - Whether or not the time to access the memory is independent or dependent of the physical location on the medium
   - RAM, Flash, ROM, SSDs are Random Access Memory (independent) while physical media is sequential access (dependent) (note: this is why we buy SSDs for computers)
- Write-ability - Can the medium be written to.
   - All mediums can be written to once. ROMs and optical media cannot be written to after it's creation (with some caveats)
- Access Time - self-explanatory, how much time will it take to access a give piece of memory.
   - RAM fast tape slow. See the pyramid chart.
- Size - The amount of data a medium can store.
  - As a drive gets slower the more data it can store (most of the time).

## Lecture 16/17
### - Computer I/O -
I/O stands for input/output. Computers need to talk to the outside world. This is how they do it.

There are two main ways this is done:
- Memory-mapped I/O - devices are mapped to the same address space data memory and program memory is.
- Port-mapped I/O - devices use dedicated address spaces and are accessed using special instructions

MMIO (Memory-Mapped I/O) is used in the same way memory is accessed, making the implementation much easier than port mapping. The main advantages stem from the lack of dedicated instructions being necessary but the disadvantage is then the entire address bus needs to be decoded.

PMIO (Port-Mapped I/O) is the opposite, with the primary benefits being less decoding being necessary and overall, less cost.

MMIO is the most commonly used method, but PMIO is used in x86 architectures. 

### - Interrupts -
The next question is if those devices are connected to a processor, how does the processor talk to them?

The process which this is done is essentially the device interrupting the flow of a program to be handled by the OS. This is called a **Interrupt** and is the process in which a device requests service from the OS or has an error. The service that handles the device is called an Interrupt Service Routine (ISR).

There are two types of interrupts, and each is handled differently:
- Maskable Interrupts - interrupts that can be disabled via flags. Most common type.
- Non-maskable Interrupts- cannot be masked or disabled. Events are always services. Reserved for critical system events.

Beyond these, there are two more subsets:
- Non-vectored Interrupts - One common request line for all devices (kind of like UDP in networking). Priority of device determined by polling order.
- Vectored Interrupts - Independent request lines with designated request and acknowledgement lines. (TCP in networking). Hardware supported for variable management solutions and thus dynamic priority.

Because of the way vectored interrupts are designed, they need a way of determining the proper ISR to use. This is handled via a Interrupt Vector Table (IVT). This organizes the type of interruption by the vector number coming from the device (where it is connected).

### - Exceptions -
Exceptions use a similar method of interrupting a process but only occur on a system malfunction during program execution. This is usually caused by an illegal action trying to occur. Internal signals determine when this happens, and a special subroutine called an exception handler is called to either terminate the program or repair the process. This is usually done in the control unit.

### - Traps - 
A system call by the processor. Occurs when the program needs privileged instructions in order to perform an operation. Internal signal occurs, OS handles them not hardware or executing program.

## Lecture 18/19/20/21/22
ok so im combining as much about caches into one section as i can to keep this a bit more organized

only the first part of this (lecture 18) is covered on midterm 2, and i will include a cutoff for that

### - Caches -
Memory is slow when compared to registers. Other storage solutions are worse, but DRAM is pretty slow, with actions taking ~100 ns to occur. When compared to registers almost instantaneous access, this is unacceptable and slows performance due to the processor needing to be stalled. The solution to this is to put a block of memory right next to the processor made of extremely fast response SRAM which have very low latency.

This is called a cache and is used to make processors faster by reducing slow memory calls. 

This equation is for access time of memory after accounting for miss rate (You will see this equation a lot so be familiar with it)

EMAT = Effective Memory Access Time

EMAT = Tsram + (Msram * Tdram)

### - Locality - 
This is kind of a weird topic because it fits in with a few topics and we're only talking about it now, but locality is important when determining the most efficient way to store data. The goal of a memory system is to get the fastest read/write speed while reducing the cache miss rate. This is what the principle of locality is.

There are two main forms:
- Spatial Locality: The idea that if a program accesses data at a address, it will most likely access data that is nearby as well.
- Time Locality: The idea that if a address is called once, it will be called again in the near future as it is more likely to be used again.

In execution, these concepts are why we copy information to a cache and why we use blocks to describe what should be copied.

Something that should also be mentioned here is layered caches. Layered caches exist to solve the cost problem processors have with caches in terms of real estate and monetary value. Layered caches just add more levels between the registers and DRAM. (this will most likely not be tested on because single processor systems don't really do this but that’s beside the point. [If you want a idea of why its really complicated](https://fuse.wikichip.org/wp-content/uploads/2018/04/2018-conf-amd-ccx.png). This image shows how a L2 and L3 cache are split up on a multi core processor and if you understand why they do this then you don't need this class.)

### - The Basics -
Blocks are the smallest unit a cache knows. There are ways to distinguish how large a block is but that is determined based on parameters from the processor and memory. Blocks do not need to be the same size as words, and can comprise many bytes/words. Words are a unit of memory access while blocks are a unit of memory transfer.

The main idea as we covered earlier is that a cache is a copy of some data in memory which is in a place that is faster to access. The way we know if data is in the cache is via a cache lookup. A cache hit is when it is found and a miss is when it is not. A cache hit directly forwards the data to the processor and a miss penalty means the processor will stall while the main memory is accessed and copied to cache and the processor. The probability of not finding data in the cache is called the miss rate and is what is mentioned in the equation above. 

Another way of describing the equation above is EMAT = Tc + (Mc * Tmm) where Tc is Time to access the Cache, Mc is Miss rate of Cache, and Tmm is Time to access Main Memory. (This also works on layered caches).

There are some really good examples of a system in the slides that can explain this way better than I can here so please go read those (start at slide 32 in lecture 18).

### - Memory Organization -
Blocks are cool, but how do we determine what goes in a block and where it goes. The memory address can be partitioned for this exact reason. Each cache will explicitly say the block size, and with this information you can calculate the number of bytes in a block (in binary) and the same number of bits it takes to represent that is the byte number the memory is representing while all of the rest are the block number. Here, just look at this:

![wow so cool](https://i.imgur.com/ZZOlAWO.png)

### - Types of Caches -
You only need to know the basics for this midterm, but you need to know these for the final. There are three types of caches:
- Fully-associative: Each memory block can be mapped to any cache block.
- Direct-mapped: Each memory block is mapped to exactly one cache block.
- Set-associative: Each memory block is mapped to a set of cache block.

### - How this works in a pipeline -
There are two stages where memory is accessed: IF and MEM. If data is not found in the cache, the processor is stalled. If it misses in the IF stage, NOPs are placed in the pipelined while the I-cache is filled. If it misses in the MEM stage, the processor is frozen and NOPs are inserted after (in WB). This is horrible for processor performance and changes it like:

Perfect:
ET = IC * CPIavg * T

Non-Ideal:
ET = IC * (CPIavg + NumMemStallsAvg) * T
CPIeffective = CPIavg + NumMemStallsAvg

Improving this performance is down to reducing pipeline stalls and thus reducing miss rate. This is best accomplished by increasing block size and increasing associativity (but only to an extent).

## Stop here if studying for midterm 2


