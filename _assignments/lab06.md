---
layout: assignment
due: 2022-04-18 23:59:59 -0800
github_url: https://www.cs.usfca.edu
permalink: assignments/lab06.html
---

{% assign img_base = site.url | append: site.baseurl | append: "/assets/img" %}

## Requirements

For this lab you should implement all the components and the top-level partial processor circuit described in the [Project 06 Guide Part 1](project06-1.html) with the following exceptions: the Extender and Data Memory for this lab.

In summary you need to implement:
1. A Program Counter using  a 32-bit Register with `CLR`. You may use your own register, or the Digital Register component
1. A Register File that has 15 registers. Two registers can be read, and one can be written, in a single clock cycle. The PC will be passed through as `r15`.
1. An ALU that supports `add`, `sub`, `mul`, `mov`, `lsl`, and `lsr`.

Your top-level processor must have a variation of the dashboard view using splitters, tunnels, and probes as shown in the guide. You do not have the replicate this view identically, but it should show the same information. You are free to come up with new and better ways to display the same information.

Your top-level circuit should be able to index through a program in instruction memory showing each instruction word for the specified program, although this is not tested by the autograder in this lab.

By manipulating the inputs to the Register File, ALU, `CLK`, and `CLR`, your circuit should be able to execute four small programs:

1. `add r0, r0, #1` resulting in `R0` = 1 
1. `mov r1, #2` resulting in `R1` = 2
1. `sub r0, r0, #1` resulting in `R0` = -1 (0xFFFFFFFF)
1. `mov r0, #1` resulting in `R0` = 1  
`mov r1, #1` resulting in `R1` = 1  
`sub r0, r0, r1` resulting in `R0` = 0

Your ALU should be able to subtract A - B and calculate the correct values for its outputs `N` (negative) and `Z` (zero)

For the autograder to test the four cases above:
1. Your main circuit must be named `lab06.dig`, have inputs named `CLK`, `CLR`, `RR0`, `RR1`, `WR`, `WE`, `ALUSrcB`, `ALUOp`, and `Imm`, and outputs named `R0` and `R1`
1. Your ALU must be named `alu.dig`, have inputs named `A`, `B`, and `ALUOp`, and outputs named `N` and `Z`

## Given

You may use any of Digital's built-in components, or your own if you prefer.

## Rubric

80 pts : Item 6 subparts 1-3  
90 pts : Item 6 subparts 1-4  
100 pts : Item 6 subparts 1-4 and Item 7