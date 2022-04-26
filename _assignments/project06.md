---
layout: assignment
due: 2022-05-02 23:59:59 -0800
github_url: https://classroom.github.com/a/eInC4Lj2
permalink: assignments/project06.html
---

## Requirements

1. You will implement a single-cycle microarchitecture for a subset of the ARMv7 instruction set architecture in Digital
1. You may use any of Digital's library of components
1. You need to pass unit tests and complete program tests provided below. 
1. You also need to get your Project02 `sort`/`find_max_index` and your Project03 `merge`/`merge_sort` implementations. Note that the complete program requirements are the same as for Project04. 
1. The unit tests will help you incrementally build and test your processor. 
1. We will provide a starter instruction memory with the unit tests and the complete program tests from Project04. 
1. You will need to add your `sort`/`find_max_index` and `merge`/`merge_sort` to the given instruction memory.

In order to make your programs runnable on your processor you must do the following:
1. You will add an assembly language main to set up at least five parameters for your functions
1. You will add the end marker (`add r0, r0, #0`) to indicate when the program should stop
1. Ensure that you do not have `.global` or `.func` directives in your programs

Your processor implementation will include the following major sub-circuits:
1. The Program Counter (PC) will be one 32-bit register with enable (EN) and clear (CLR) inputs.
1. Machine code programs will be stored in a ROM components, just as we did in Project05. Like in Project05 your instruction memory will be able to select the program you want to execute.
1. A Register File that can support reading 3 registers in a single clock cycle: `RD0`, `RD1`, and `RD2`. This is a modified version of the Register File from Lab06 and Lab07.
1. The Arithmetic Logic Unit (ALU) will perform data processing tasks such as `add`, `sub`, `mul`, and `mov`. The ALU will also compute NZCV values for subtraction (as a result of a `cmp` instruction).
1. The Sign Extension Unit (Extender) will perform 8-bit zero extension, 12-bit zero extension, and 24 bit sign extension (and multiply by 4).
1. A Shift Unit (Shifter) will support shifts in `mov` and SDT instructions.
1. Data Memory. We will use a Digital RAM component for data memory. This will be used for stack memory by our test programs. We will configure the RAM component to hold 32 bit word values. This will make it easy to support `ldr` and `str`. 
1. The Control Unit will decode machine code instructions and support conditional branch execution. As with Project05, the Control Unit will be the most complex part. 
1. The Data Path will connect data between the various sub-circuits
1. The Control Path will connect the Control unit to various sub-circuits and multiplexers

## Given
1. We have compiled a three-part guide to completing this project: [part1](project06-part-1.html), [part2](project06-part-2.html), [part3](project06-part-3.html)
1. We will discuss the major sub-circuits in lecture and you will have hands-on time to develop and ask questions
1. The GitHub assignment has a starter repo which contains:
    - A starter instruction memory circuit 
    - Assembly language implementations for test cases 0-18
    - A Python script for creating a decode ROM hex file

Unit  Tests
- 00-dp-reg
- 01-dp-imm
- 02-dp-lsl-imm
- 03-dp-lsr-reg
- 04-dp-asr-imm
- 05-mem-reg
- 06-mem-imm
- 07-mem-shift-reg
- 08-blbx
- 09-b
- 10-beq
- 11-ble
- 12-bgt
- 13-mul
- 14-rsb

Complete Program Tests
- 15-quadratic
- 16-get_bitseq
- 17-max3
- 18-fib-rec

Your Programs
- 19-sort_s (and find_max_index_s)
- 20-merge_sort_s (and merge_s)

## Rubric
For interactive grading you should be able to run the autograder tests and manually execute your `sort_s` and `merge_sort_s` programs.

- (30 points) Unit tests
- (20 points) Complete program tests
- (9 points) Your `sort_s`
- (9 points) Your `merge_sort_s`
- (10 points) Interactive grading question #1
- (10 points) Interactive grading question #2
- (12 points) Neatness, including
    - Your decoder PDF
    - A debug dashboard similar to (or better than) the given top-level circuits
    - A legend for program numbers so we can observe the results in interactive grading

## Extra Credit
(2 points) Add support for `ldrb` and `strb`. You need to demonstrate you can execute `is_pal_rec_s` and `to_upper_s`.