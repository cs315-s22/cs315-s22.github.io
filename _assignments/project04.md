---
layout: assignment
due: 2022-03-21 23:59:59 -0800
github_url: https://www.cs.usfca.edu
permalink: assignments/project04.html
---

## Requirements 
1. You will write an emulator in C for a subset of the ARMv7 Instruction Set Architecture (ISA). 
1. You do not have to emulate the entire instruction set; just enough to emulate `fib_rec`, `get_bitseq`, `is_pal` , `max3`, `merge_sort`, `quadratic`, `sort`, and `to_upper`. You will use your implementation of `sort/find_max_index` and `merge/merge_sort`. The other assembly implementations are given.
1. Your emulator will need the logic (decoding and emulating instructions) and state (`arm_state`) from lab04
1. Your emulator will support dynamic analysis of instruction execution. Here are the metrics you will collect.
    1. \# of instructions executed (i_count)
    1. \# of data processing  and mul instructions executed (dp_count)
    1. \# of SDT (memory) instructions executed (sdt_count)
    1. \# of branch instructions executed including b, bl, bx, bCC (b_count)
    1. \# of branches taken including b, bl, bx, bCC (b_taken)
    1. \# of branches not taken from bCC (b_not_taken)
1. Your emulator will include an implementation of a 4-way set-associative cache, as is common in commercial processors. We are giving you an implementation of a direct mapped cache. In addition to the set associative cache implementation, you will collect the following metrics:
    1. Total memory references (refs)
    1. Hits (hits)
    1. Misses (misses)
    1. Cold misses (misses_cold)
    1. Hot misses (misses_hot)

## Given
1. In lecture and lab, we will: 
    1. illustrate how to decode machine code and execute the operations specified
    1. illustrate a direct-mapped cache and describe the data structures and algorithms required for a set-associative cache
1. In-class coding will be pushed to Github. You will provide the rest of the code yourself
1. We will provide autograder test cases for the emulation targets

## Grading Rubric
**Automated testing**
80 pts: Automated tests 

**Interactive grading**
10 pts: code walkthrough, including, but not limited to, your implementation of dynamic analysis and the instruction cache.

**Coding Style**
10 pts: Clean repo, consistent naming and indentation, no dead code, no unnecessarily complex code