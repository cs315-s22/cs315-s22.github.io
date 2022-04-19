---
layout: assignment
permalink: assignments/project06-part-2.html
---

{% assign img_base = site.url | append: site.baseurl | append: "/assets/img/project06" %}

![extender]({{ img_base }}/extender.png)

The extender will perform sign extension of 8- and 12-bit unsigned immediate values (that is, zero extension out to 32 bits) and 24 bit signed branch offsets (that is, test bit 23 and extend its value out to 32 bits)

Inputs
1. `iw` (32 bits) the current instruction word
1. `ExtOp` (2 bits) the extender selector, which selects between
    - 00: 8-bit zero extension. Concatenate the low 8 bits with 24 bits of 0 to output a 32-bit value
    - 01: 12-bit zero extension. Concatenate the low 12 bits with 20 bits of 0 to output a 32-bit value
    - 10: 24 bit sign extension. Recall from Project04 that the 24-bit branch offset is expressed in words, and the PC is expressed in bytes. Therefore you must multiply the sign-extended value by 4 as you did in Project04. This can be accomplished using Digital's Barrel Shifter or Multiply components.

Outputs
1. `extimm` (32 bits) the extended value

![data-memory]({{ img_base }}/data-memory.png)

1. Our programs will need access to RAM, e.g., the stack, arrays, etc.
1. We will use the component with separate load(`ld`) and store (`str`) inputs.
1. We will configure the RAM for 32 data bits, which means we can load and store only 32-bit values.
1. You will configure the number of address bits based on the amount of stack space your test programs need. Recursive functions may use quite a bit of stack space

Inputs
1. `A` (you figure out how many bits you need): the input address for the read or write to memory
1. `Din` (32 bits): the value to be written to memory when `str` is 1
1. `str` (1 bit): set to 1 if we are storing (writing) to memory
1. `C` (1 bit): clock input
1. `ld` (1 bit) set to 1 if we are loading (reading) from memory

Outputs
1. `D` (32 bits): the data value we read from memory at address `A`
1. As with the ROM, we are only supporting word addresses. Therefore, when calculating a target memory address for `ldr` or `str` instructions, we need to convert the byte address into a word address before sending it to the RAM component
1. In addition, we need to ensure that the target address is the same as the number of bits in the `A` input

## Adding Control and Data Paths

In this post and in upcoming lectures, we will discuss a methodology for identifying instructions and splitting off the bits of the instruction word to interpret what the instruction means. Once we have a circuit which represents the meaning of the instruction, we can use that to make decisions about how to select and direct data on the data path and determine the control inputs to the processor components such as the register file and ALU.

To start, let's use this short program:
```
first_s:
    mov r0, #1
    mov r1, #2
    add r2, r0, r1
    add r0, r0, #0
```
To execute these instructions we need to:

1. Add a data path for the `add` instruction word coming out of the ROM. In the schematic below, that's shown with bits 0-3 (`rm`) going into `ReadReg0`, bits 12-15 (`rn`) going into `ReadReg1`, and bits 16-19 (`rd`) going into `WriteReg`
1. Add a data path for the mov instruction. In the schematic, that's shown using the iw tunnel coming out of the ROM, and into the Extension Unit. 
1. Add an initial Decoder (Control Unit) to choose between adding registers and moving immediate data. The schematic shows the output of that decision in `ALUop`, which is the selector into the ALU.
1. Now you can execute the mov and add instructions to put 1 + 2 = 3 in r2

![control-data]({{ img_base }}/control-data.png)

## Developing the Control Unit

We will build the circuit for the control unit using the digital logic gates we learned about in Project05. To build the requirements, we will use a truth-table-like spreadsheet:

![spreadsheet-1]({{ img_base }}/spreadsheet-1.png)

Using the Instruction Set Manual,
1. We can identify bits 27 and 26 from the instruction word for data processing instructions, recall if these are set to 00, then the instruction is possibly a data processing instruction.
1. We can identify the Data Processing opcode in bits 24-21. Add is 0b0100 and mov is 0b1101.

We will use a simple model for outputs for the control unit:
1. Since the Register File has a `WriteEn` control input, let's make up a `RFW` (Register File Write) output to match it up with. `RFW` will be 1 when we identify an instruction which needs to write to the register file
1. Since the Extender needs a control input to select its operations, let's make up an `EXTop` output to match it up with. The values coming out of EXTop will be
    - `0b00`: 8-bit zero-extended immediate
    - `0b01`: 12-bit zero-extended memory address
    - `0b10`: 24-bit sign-extended branch offset
    - `0b11`: 5-bit zero-extended shift immediate
1. Since the ALU needs a control input to select its operations, let's make up an `ALUop` to select arithmetic instructions in the ALU. The values coming out of ALUop will be:
    - `0b000`: addition
    - `0b001`: subtraction
    - `0b010`: multiplication
    - `0b011`: move
1. Since we need a way to differentiate between ALU operations which use registers vs. those which use immediate values, we will make up a data output to send to the Extender Unit, and call it `ALUsrcB`

This schematic shows how to construct gates which match the spreadsheet above, and can execute the `first_s.s` program above. You will add instructions to the spreadsheet, which may involve new inputs and outputs, and add build a circuit like this within your Control Unit

![control-v1]({{ img_base }}/control-v1.png)

## Adding Support for `bl` and `bx`

Now that you can execute `add` and `mov` instructions, let's move on to a version which supports `bl` and `bx`, as implemented in `first_main.s`

```
main:
    mov r0, #1
    mov r1, #2
    bl first_s
    add r0, r0, #0
first_s:
    add r0, r0, r1
    bx lr
```

To support branch, we need to modify the data path and the control path, as well as the control unit. In the schematic, that's shown with the branch multiplexer choosing PC + 4 or a branch target calculated by the ALU

See the following new top-level processor circuit with an updated datapath and control path. Note that we have added several MUXes that allow us to direct traffic. For example, 
1. The MUX control by `PCsel` will choose which value to send to the PC. For normal instructions this will be PC+4, for `b` and `bl` this will be the Branch Target Address that is computed using the ALU, and for BX. 
1. The MUX controlled by `RR0sel` now chooses between `rn` for data processing instructions and `0xF` (the PC) for the branch target calculation. 
1. The MUX controlled by `RR1sel` has been added for supporting future instructions not currently supported in this version. 
1. The MUX controlled by `WRsel` chooses between `rd` and `0xE` (`LR`) for which register to write to 
1. The MUX controlled by `WDsel` chooses between the ALU Result and PC+4 as for input into `WriteData`.

![muxes]({{ img_base }}/muxes.png)

To support these instructions, we need to also evolve the Decoder (Control Unit) to control the new MUXes and new components. The spreadsheet below shows new inputs and outputs for these instructions. Note that an X input means "don't care" -- that is it could be 0 or 1. So we can ignore "don't care" values when detecting an instruction type.

![spreadsheet-2]({{ img_base }}/spreadsheet-2.png)

When we identify branch instructions those will output the PC value on `RD0`, and choose the next PC value coming from the ALU result on the data path. That result will be the branch target address. `bx` chooses the value in the register number found in bits 3-0 (`rn`).

This table adds rows for adding an immediate value and moving a register value. It also adds new control lines for `bl` and `bx`. The schematic below shows how to add those inputs and outputs to our Control Unit circuit. Instead of using constant values and a MUX for the output bits, we can use a ROM instead. In this way you can copy the output bits from the spreadsheet into the contents of the ROM. In this example, the ROM has 3 address bit and 16 data bits. When editing the ROM contents you can cut and paste the output bit column and paste it directly into the contents.

![decode-rom]({{ img_base }}/decode-rom.png)
