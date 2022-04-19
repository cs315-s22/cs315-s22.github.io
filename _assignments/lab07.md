---
layout: assignment
due: 2022-04-25 23:59:59 -0800
github_url: https://classroom.github.com/a/UAmrZDLI
permalink: assignments/lab07.html
---

## Requirements
1. For this lab you will combine your work from lab06 with a new implementation of the Control circuits as described in the [Project 06 Guide Part 2](project06-part-2.html)
1. Please use an incremental development approach and commit two top-level circuits: `lab07_part1.dig` and `lab07_part2.dig`. Each top-level circuit should contain its own decoder circuit identify instructions and propagate the appropriate control signals. 
1. Use the spreadsheet approach to develop pseudo-Boolean algebra for the inputs (e.g. `add(r)`, `add(i)` etc.) and outputs (e.g. `ALUOp`, `ExtOp`, etc.) for the circuit, as described in lecture.
1. You can start by using 8 bits of input and 16 bits of output for the Data Processing instructions. You may need to add more as you add other instruction families.

## Part 1
Build the control unit and top-level processor circuit which can execute this program (also given in the Guide)
```
first_s:
    mov r0, #1
    mov r1, #2
    add r2, r0, r1
    add r0, r0, #0
```

## Part 2
Build the control and top-level processor circuit which can execute this program (also given in the Guide)
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

## Given
1. You may use any of the circuits shown in the Project06 Guide Part 2.
1. You may use any of Digital's built-in components, or your own if you prefer.

## Rubric
80 pts: passes automated tests for part 1  
100 pts: passes automated tests for both parts