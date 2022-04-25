---
layout: assignment
permalink: assignments/project06-part-3.html
---

{% assign img_base = site.url | append: site.baseurl | append: "/assets/img/project06" %}

![shift-unit]({{ img_base }}/shift_unit.png)

**Shift Unit**

To support the implementation of `lsl`, `lsr`, and `asr` as well as SDT with a shift, e.g., `ldr r0, [r1, r2, LSL #2]` we will need a shift unit. The shift unit will have three inputs (`iw`, `Rm`, `Rs`, `SHsel`) and one output `R`:
- `iw` - The 32 bit instruction word. We need bits 4-11 which will be extracted in the Shift Unit.
- `D` - The value to be shifted.
- `Rs` - The shift amount if a register shift amount is selected
- `SHsel` - Choose to propagate `D` unchanged or `D` shifted as determine by bits 4-11.
- `R` - The output value

You will put the shift unit in front of the `ALUsrcB` MUX.

**Conditional Execution**

1. In order to support conditional branch instructions (e.g. `beq`, `bne`, `bge`, etc.) we will create a Control Unit that embeds our decoder.
1. The Decoder identifies instructions and sets the output control lines.
1. The conditional branch circuitry added below is incomplete as well as is the decoder. You will add support for other cases as needed by your Project03 functions.
1. We will use a 4-bit register to store the CPSR bits which are the result of the subtraction operation in the ALU.
1. Recall that although the ARM architecture permits conditional execution of all instructions, you are only required to implement conditional branch instructions.

![condex]({{ img_base }}/condex.png)

To support conditional execution:
1. We need a new output from the Main Decoder which determines whether we are executing a `cmp` instruction
1. When executing a `cmp` instruction, the `EN` input to the CPSR register will be set to 1
1. The splitter extracts the `COND` bits from bits 28-31 of the instruction word
1. The comparators for `AL` (always) and `EQ` (if equal) look for the condition codes shown in the Instruction Set Manual
1. If the OR gate outputs 1, then the `PCsel` output is 1. If `PCsel` is 1, we will calculate PC + branch offset. If `PCsel` is 0, we will simply move to the next instruction, PC+4

The key intuition here is that to ignore an instruction we simply don't update the state which would have been changed by the instruction.

This will be useful if you wish to support conditional execution of other instructions besides branch. The instruction executes, but its results are ignored, as though it never happened.

**Using a Decode ROM**

In Part 2, we showed how to generate the control bits using a multiplexer fed by constant values containing the control bits as a 15-bit value.

It's more concise to put the control bits in a ROM, and use the output of the Priority Encoder to index the ROM. Therefore each instruction decoded is an address, and the value at that ROM address is the 15-bit value containing the control bits. If your decoder has 32 or less outputs, your ROM will need 5 address bits (2^5 = 32). Your ROM will need enough data bits to hold all of the control bits.

To create the data in the ROM, you can copy/paste the concatenated control bits from your spreadsheet into a `.hex` file, and load that into your ROM. While you could just type directly into the ROM's data editor, we suggest creating a `.hex` file to minimize work when you add control bits or find bugs in your spreadsheet. Example of `.hex` file format:

```
v2.0 raw
8000
9600
50A0
D0AC
100
```

![decode-rom]({{ img_base }}/decode_rom.png)

We provide you with a Python script (`mkdecoderom.py`) that makes is easy to create the .hex for the decode ROM. First, create a file that contains the binary versions of the output control bits from your spreadsheet. You can create the decode ROM hex file like this:

```
$ python3 mkdecoderom.py > decode_rom.hex
<paste your output control bits column here>
^D
$
```