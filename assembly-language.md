# Assembly Language and Binary Exploitation

## Architecture

### Assembly Language

When we program using high level languages such as C++ or Python, we are using human friendly languages which are easier for us to understand.

The hardware of computers, however, cannot understand human languages and therefore there needs to be low level languages such as assembly.

The processor of a computer works with binary so only combinations of 0 and 1

This we can call *binary machine code* - an example of this is:

`01001000 10000011 11000000 00000001`

This can also be represented as *machine shellcode* which is a *hexadecimal* representation of the binary machine code:

`4883C001`

As can be seen, for a human to use these languages to complete complex algorithms is difficult.

>[!NOTE]
>Processors use different electrical charges for 0 and 1 which is why we can use binary data to interact with them

Assembly language is a more human readable form of the same instructions which correspond one-to-one with the machine code:

`add rax, 1`

#### High Level and Low Level Languages

Since there are different types of processor which understand different machine code, we need high level languages to help us write programs which can work on different hardware.

A *compiler* enables us to take code from a language such as C++ and compile it so that it works for a specified processor. This means that we can write code in a language such as C++ and then compile it into different assembly languages which is then assembled into machine code which will work on different processors.

There are also *interpreted* high level languages such as Python and JavaScript. These languages are not compiled - rather, they are interpretted at their run time. These languages use libraries which are written in compiled languages such as C++ to run their commands on the processor.

### Computer Architecture

Most modern computers use *Von Neumann Architecture* which is named after Von Neumann who developed it in 1945. This architecture was developed to enable the creation of general purpose computers which is the name Alan Turing gave to them. This idea was in turn based on Charles Babbage's programmable computer idea from the mid 1800s.

Von Neumann Architecture is designed to execute instructions as machine code from given algorithms.

This architecture is comprised of the following components:

- Central Processing Unit (CPU)
- Memory Unit
- Input and Output Devices (Mass Storage Unit, Keyboard, Display)

The CPU itself can be broken down into pieces:

- Control Unit
- Arithmetic / Logic Unit (ALU)
- Registers

When we use assembly language, we are communicating mostly with the cpu and memory (cache and RAM).

#### Memory

We can think of this as the *primary* memory of a computer.

##### RAM

All the data and instructions for a program are loaded into the RAM when it is executed.

The CPU can then access the data and instructions of the program when it needs it. The reason for this is that accessing data directly from a *secondary* memory storage unit such as a hard-drive takes much longer since it further from the CPU.

RAM is therefore *volatile* memory - this means that the data and instructions of a program are stored in it when they are needed - once the program exits its execution the data and instructions are removed from the RAM so the memory space is free for other applications to use it.

>[!NOTE]
>32 bit gives us four gigabytes of RAM storage for memory addresses whilst 64 bit gives us 18.5 exabytes

Inside the RAM there are four main sections:

- Text
- Data (Data and .bss)
- Heap
- Stack

The text segment is lowest in memory whilst the stack is the highest in memory. The stack grows down as instructions are added to it whilst the heap grows up.

When a program is executed, it will receive its own text, data, heap and stack as *virtual* memory.

| Text | Data | Heap | Stack |
| --- | --- | --- | --- |
| Assembly instructions are loaded here to be used by the CPU as needed | The Data part holds variables whilst the .bss part is space for unassigned variables | Used to store data - larger and more versatile than stack | Holds data - can only be accessed in order - Last In First Out - think Pringles!

RAM is faster than secondary memory units such as hard-drives, but it takes more instructions to access data from it than from the *cache* and is therefore slower than it.

##### Cache

The cache memory is often found in the CPU itself. This means it is faster than RAM because it is running at the same *clock speed* as the CPU.

The cache is therefore used by the CPU to access the upcoming instructions and data.

There are three levels of cache memory - though not all CPUs use the third level:

| Level 1 | Level 2 | Level 3 |
| --- | --- | --- |
| L1 is located in each CPU core and is the fastest cache memory | L2 is very fast and shared between all CPU cores | L3 is larger than L2 but slower than it |

>[NOTE]
>The levels are determined by how close they are to the CPU - L1 is the closest and therefore the fastest

##### Memory Speed

Memory which is closer to the CPU is faster and memory which holds less data is faster. An example of speed is receiving an instruction. For a CPU to do this from the *registers* takes one clock cycle. To do the same from L1 cache memory takes several clock cycles. To do the same from RAM takes about 200 clock cycles. Considering that instructions are being fetched billions of times every second, it becomes clear how much difference it makes to performance to fetch data and instructions from different memory storage units.

#### Input and Output Devices Including Storage

The input and output devices are hardware such as keyboards and screens. The secondary memory mass storage units - hard-drives for example - are also included in this part of Von Neumann architecture.

*Bus Interfaces* are used by the processor to access and control the input and output devices. These bus interfaces are pathways for data to move along using electrical charges which correspond to *bits*.

Bus interfaces have a maximum capacity of bits they can carry at the same time. The maximum is 128 bits.

##### Secondary Storage

The secondary storage units of computers store *non-volatile* data - this means that the data is retained even when the system is shut down and loses electrical power. Traditionally, this was accomplished using magnetism on tapes and hard-drive discs. We can see this in terms of binary data being stored using magnetic polarity. This method has since mostly but not wholly replaced by Solid State Drives which are faster since they are more similar to RAM.

Since secondary storage units are further away from the CPU and hold much more data, they are far slower than other memory types. This is why data is transfered from them into the RAM when a program is executed.

Data from secondary storage devices is accessed via bus interfaces such as *SATA* and *USB*

### CPU Architecture

The CPU is in charge of executing the instructions given to the computer in machine code. The CPU has a control unit which moves and controls data. It also has an arithmetic / logic unit which is responsible for calculations and logic.

There are different Instruction Set Architecture (ISA) for different types of CPU.

The ISA is the set of instructions which a programmer or compiler must understand and use in order to write a program which works as intended for a specific CPU.

There are two main ISAs - CISC and RISC

CISC architecture uses more complex instructions which means that programs written for CPUs using it require fewer instructions - it optimises the number of instructions over their complexity.

RISC architecture uses more simple instructions which means that more of them are needed - it optimises simplicity over the number of instructions needed.

#### Clock Speed and Clock Cycle

Each CPU has a clock speed which determines how fast it is. One tick of a clock starts a *clock cycle* which processes a simple instruction.

The number of *clock cycles* which can be run every second is measure in *Hertz* - a *clock speed* of 2.0 GHz means that the CPU can run two billion *clock cycles* every second for each core the CPU has.

>[!NOTE]
>Modern CPUs have multiple cores which can run clock cycles at the same time

#### Instruction Cycle

This is a cycle which is how long the CPU takes to process one machine instruction.

Each instruction cycle has four stages: Fetch, Decode, Execute and Store.

One instruction cycle will typically take several clock cycles to finish. The exact amount of clock cycles taken for an instruction cycle to complete depends on the ISA of the CPU as well as the complexity of the instruction which is being executed.

>[!NOTE]
>Modern CPUs can process multiple instructions at the same time because they have more than one thread and core

#### Processor Specific Instructions

Two processors which use different ISAs will interpret the same machine code differently. For example, the machine shellcode `4883C001` will be interpretted by a CPU which uses *x86_64* architecture as `add rax, 1` whereas a CPU which uses *ARM* architecture will interpret it as `biceq r8, r0, r8, asr #6`

>[!NOTE]
>One type of ISA can have different syntax for it - for example x86 architecture can use *Intel* syntax such as `add rax, 1` *and* it can have *AT&T* syntax which would use `addb $0x1,%rax`

