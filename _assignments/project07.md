---
layout: assignment
due: 2022-05-12 23:59:59 -0800
github_url: https://classroom.github.com/a/kCJjmW4E
permalink: assignments/project07.html
---

## Requirements 
1. For this project, you will start with a given, basic implementation of a pipelined processor. This is a 5-stage pipeline processor that includes pipeline registers between each stage. To execute code on this processor, code must contain `nop` (no operation) instructions between real instructions so that the register file is updated properly. We will learn how this processor works in class this week.
1. You will evolve the given implementation to include a Hazard Unit and data path additions which provides support for Register Forwarding for data dependencies, Stalling for memory reads, and Flushing the pipeline to handle branches. The ultimate goal is to remove the need for `nop` instructions to properly execute code and to allow the pipeline to run at the fastest rate possible.
1. You will commit the entire implementation of your pipelined processor, including the given circuits and ROMs, to your assignment repo. Your top-level processor must be named `project07.dig`.
1. The Hazard Unit gives the processor the ability to automatically handle the hazard conditions described in the following test cases.

    **04-dp-fwd.s**

    ```
    main:
        mov r1, #1
        mov r2, #2
        add r0, r1, r2   @ r0 should be 3
        add r0, r0, #0   @ marker instruction
    ```
    This program illustrates a Data Hazard, since `r0` depends on the Writeback stage of the two immediate-form `mov` instructions. Your Hazard Unit will enable Register Forwarding so that the values of `r1` and `r2` are available when the `add` instruction begins the Execute stage

    **05-ldr-stl.s**

    ```
    main:
        mov r0, #0
        mov r1, #1
        str r1, [r0]
        ldr r2, [r0]
        add r0, r2, #1   @ r0 should be 2
        add r0, r0, #0   @ marker instruction
    ```
    This program illustrates the need to Stall the pipeline, since the `add` instruction depends on the value loaded by the `ldr` instruction. Your Hazard Unit will enable a Stall in the appropriate Pipeline Registers.

    **06-blbx-fls.s**

    ```
    main:
        mov r0, #2
        bl foo
        add r0, r0, #0  @ marker instruction
    foo:
        add r0, r0, #4  @ r0 should be 6
        bx lr
    ```
    This program illustrates a Control Hazard. Since the `bl` instruction means that subsequent instructions (the marker) should not be executed, we need to Flush the pipeline.

## Given

1. You may use any of the circuits shown in lecture, including the pipeline registers and disassembly circuit.
1. `project07-given` includes a simplified single-cycle processor including:
    1. Support for RAM including `ldr(i)` and `str(i)`
    1. An implementation of the ASCII-based disassembler, including the decoder which outputs the inum to match the disassembler's ROM.
    1. Instruction memory, including assembly source and hex files, for:
        1. Four programs which execute using `nop` instructions to manually avoid hazards
        1. The three programs which require your Hazard Unit to execute correctly
1. You may use any of Digital's built-in components, or your own if you prefer.

## Rubric

80 pts: passes automated tests for the `04-dp-fwd` test case  
90 pts: passes the automated tests `04-dp-fwd` and `05-ldr-stl`   
100 pts: passes the automated tests for `04-dp-fwd`, `05-ldr-stl`, and `06-blbx-fls`

## Extra Credit

1. 1 pt: Add support for conditional branch, providing a test program to demonstrate your work

