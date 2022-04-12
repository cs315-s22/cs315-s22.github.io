---
layout: assignment
permalink: assignments/project06-1.html
---

{% assign img_base = site.url | append: site.baseurl | append: "/assets/img/project06" %}

## Background

The project spec for project06 provides high-level requirements. This guide, along with lecture material, will drill down into the details. Using all of this information, you will implement and simulate a single-cycle ARM processor in Digital

## Major Components

![components]({{ img_base }}/components.png)

1. The PC register will hold the address of the instruction we are currently executing
1. The Instruction Memory holds the machine code representation of our test programs, including matches_s and merge_sort_s from Project03.  
1. You will use the hex output from makerom3.py as input into instruction memory
1. The Register File supports reading and writing from ARM registers. We will expose the register values as outputs, so they may be used as inputs to other circuits
1. The Arithmetic Logic Unit (ALU) supports addition, subtraction, multiplication, logical shift left, and logical shift right.  The ALU also computes the condition code bits N, Z, C, V for supporting conditional execution. We will use the ALU for branch target calculation.
1. The Extender supports zero-extension of 8- and 12-bit unsigned immediate values, as well as sign extension of 24-bit branch targets. 
1. Data Memory is a Digital RAM component which supports our simulated stack, storing function parameters and preserved register values, as needed.

## PC

1. We will represent PC using a Digital Register component, in the top level of our processor
1. Digital does not provide a CLR input to its registers, but we can add one as our project grows
1. EN will always be 1 so we can move the PC forward 4 bytes on each CLK cycle
1. As in Project04, you will update the PC with a calculated address for BL and BX instructions
1. The D output will address the Instruction Memory to retrieve the instruction word at the PC address

## Instruction Memory

1. Instruction memory will be built out of Digital ROM components, starting with one and expanding to more for each simulated function
1. Each ARM machine code instruction is 32 bits, so the ROM will have 32 data bits
1. The number of address bits will vary based on the size of your simulated programs. Recall that with 8 address bits, you can address 2^8 = 256 instructions
1. Similar to Project05, note that the ROM is addressed by 32-bit words, but the PC represents a byte address. You can obtain the word address using a splitter
1. You will use `makerom3.py` to generate a `.hex` file which can be loaded into the ROM.

## Register File

![register-file]({{ img_base }}/regfile.png)

You will create a circuit which supports reading from two registers and writing to one register in the same clock cycle.

**Inputs**
1. ReadReg0 (4 bits) selects the register value to output on RD0
1. ReadReg1 (4 bits) selects the register value to output on RD1
1. WriteReg (4 bits) selects the destination register to update
1. WriteEn (1 bit) determines whether we will update a register in this clock cycle
1. WriteData (32 bits) contains the value we will write to WriteReg if WriteEn is high
PC (32 bits) contains the value of the PC so register 15 (PC) can be selected on RD0 or RD1. Hint: you will want to input PC + 8 when computing the branch target address for a branch instruction
1. CLK (1 bit) is the clock input from the top-level clock component
1. CLR (1 bit) allows the register values to be set to zero on a clock tick

**Outputs**

RD0 (32 bits) contains the value of the register specified by ReadReg0
RD1 (32 bits) contains the value of the register specified by ReadReg1
r0-r15 (32 bits) are outputs for the 16 registers (including the PC which is passed through).
You should use these as inputs to tunnels which are used to build the dashboard on the top-level circuit.
You should also reflect PC and PC+8 on the dashboard using tunnels, since that will be useful for debugging branch calculation

## Extender

![extender]({{ img_base }}/extender.png)

The extender will perform sign extension of 8- and 12-bit unsigned immediate values (that is, zero extension out to 32 bits) and 24 bit signed branch offsets (that is, test bit 23 and extend its value out to 32 bits)

**Inputs**

1. iw (32 bits) the current instruction word
1. EXT (2 bits) the extender selector, which selects between
1. 00: 8-bit zero extension. Concatenate the low 8 bits with 24 bits of 0 to output a 32-bit value
1. 01: 12-bit zero extension. Concatenate the low 12 bits with 20 bits of 0 to output a 32-bit value
1. 10: 24 bit sign extension. Recall from Project04 that the 24-bit branch offset is expressed in words, and the PC is expressed in bytes. Therefore you must multiply the sign-extended value by 4 as you did in Project04. This can be accomplished using Digital's Barrel Shifter or Multiply components.

**Outputs**

1. extimm (32 bits) the extended value

## Arithmetic Logic Unit (ALU)

![alu]({{ img_base }}/alu.png)

1. The ALU is a combinational logic component. It does not hold state like sequential logic components, so it does not need a CLK input.
1. Data processing instructions use addition, subtraction, multiplication, logical shift left, and logical shift right.
1. Load/store memory instructions use addition and subtraction to calculate target memory addresses
Branch instructions use addition to calculate the target branch address

**Inputs**

1. A (32 bits) is the first ALU operand
1. B (32 bits) is the second ALU operand
1. ALUop (3 bits) selects an ALU operation:
    ```
    0b000: add
    0b001: sub
    0b010: mul
    0b011: mov
    0b100: lsl
    0b101 lsr
    ```

**Outputs**

1. R (32 bits) is the ALU result
1. NZCV (4 bits) are the CPSR values computed for a `cmp` instruction. The 4-bit output will be wired to the Control Unit
1. You should also wire tunnels from NZCV to the dashboard for debugging purposes
1. If you need your ALU to support more operations, you may need to add more selector bits and connect those to the Control Unit

## Data Memory

![data-memory]({{ img_base }}/data-memory.png)

1. We will simulate our stack using the Digital RAM component
1. We will use the component with separate load(ld) and store (str) inputs.
1. We will configure the RAM for 32 data bits, which means we can load and store only 32-bit values (which is why won't be able to execute reverse_s)
1. You will configure the number of address bits based on the amount of stack space your test programs need. Recursive functions may use quite a bit of stack space

**Inputs**

1. A (you figure out how many bits you need): the input address for the read or write to memory
1. Din (32 bits): the value to be written to memory when str is 1
1. str (1 bit): set to 1 if we are storing (writing) to memory
1. C (1 bit): clock input
1. ld (1 bit) set to 1 if we are loading (reading) from memory

**Outputs**

1. D (32 bits): the data value we read from memory at address A
1. As with the ROM, we are only supporting word addresses. Therefore, when calculating a target memory address for LDR or STR instructions, we need to convert the byte address into a word address before sending it to the RAM component
1. In addition, we need to ensure that the target address is the same as the number of bits in the A input

## Initial Top-Level Circuit

1. This starter circuit gives you a general idea of how to start.
    1. It can execute some instructions, with manual help from you.
    1. In order to play with it, you can put instructions into the ROM, and manually enter the RR0, RR1, and WR inputs to the Register File and ALU.
    1. Of course the Control Unit is the big missing piece, which we will discuss separately.
1. Wire up PC = PC + 4
1. Wire explicit inputs to all components
1. Add tunnels and probes to see register state and NZCV

![project06-part1]({{ img_base }}/part1.png)
