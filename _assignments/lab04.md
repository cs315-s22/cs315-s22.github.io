---
layout: assignment
due: 2022-03-07 23:59:59 -0800
github_url: https://www.cs.usfca.edu
permalink: assignments/lab04.html
---

## Requirements
1. You will use the given code, lecture material, and ARM reference manual to develop a C program which can read ARM machine code and emulate what the instructions would do on a real processor.
1. You do not need to emulate the whole ARM processor -- just enough to execute the given assembly programs: `quadratic_s`, `midpoint_s`, `max3_s`, and `get_bitseq_s`
1. The given code provides a framework for testing that the emulator output matches the output of the C and assembly language versions of the programs
1. Your emulator will need logic, including data processing (`add`, `mov`, `sub`, etc.) and branching (`b`, `bCC`, `bl`, `bx`) s to run the programs
Your emulator will need state, including the general purpose registers and the status register (`cpsr`)
1. Example Output
    ```
    $ ./lab04 quadratic 2 4 6 8
    C: 36
    Asm: 36
    Emu: 36

    $ ./lab04 midpoint 0 4
    C: 2
    Asm: 2
    Emu: 2

    $ ./lab04 max3 3 8 5
    C: 8
    Asm: 8
    Emu: 8

    $ ./lab04 get_bitseq 94117 12 15
    C: 6
    Asm: 6
    Emu: 6
    ```

## Rubric
1. For a Check-, your lab passes quadratic and midpoint tests
2. For a Check, your lab passes quadratic, midpoint, and max3 tests
3. For a Check+, your lab passes quadratic,  midpoint, max3, and get_bitseq tests