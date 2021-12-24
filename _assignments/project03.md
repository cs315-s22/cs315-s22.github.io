---
layout: assignment
due: 2022-02-28 23:59:59 -0800
github_url: https://www.cs.usfca.edu
permalink: assignments/project03.html
---

## Requirements
1. You will develop ARM assembly language implementations of the following problems, and print the results to ensure that both the C implementation and your ARM implementation compute the correct answer.
1. Your executables must be named as follows, and must be compiled with a `Makefile`
1. We will test your projects using `autograder`

**get_bitseq**

Given a 32-bit unsigned integer, extract a sequence of bits and return as an unsigned integer. Bits are numbered from 0 at the least-significant place to 31 at the most-significant place. Here is the usage:


get_bitseq \<value> <start_bit> <end_bit> 
```
$ ./get_bitseq 94117 12 15 
C: 6
Asm: 6
$./get_bitseq 94117 4 7
C: 10
Asm: 10
```
For this problem, you are given the assembly implementation and you have to write the C implementation.

**get_bitseq_signed**

Given a 32-bit unsigned integer, extract a sequence of bits and return as a signed integer. Bits are numbered from 0 at the least-significant place to 31 at the most-significant place. When computing the signed return value you can assume the end_bit is the most significant bit of the signed bit range. You need to sign-extend this value to become a 32-bit 2's complement signed value. Here is the usage:

get_bitseq_signed \<value> <start_bit> <end_bit> 

```
$ ./get_bitseq_signed 94117 12 15 
C: 6
Asm: 6
$./get_bitseq_signed 94117 4 7
C: -6
Asm: -6
```
For this problem, you are given the assembly implementation and you have to write the C implementation.

**merge**

Given the array length followed by an array containing two sub-arrays in sorted order, merge combines the sub-arrays
```
$./merge 6 1 3 5 2 4 6
C: 1 2 3 4 5 6
Asm: 1 2 3 4 5 6
```

**merge_sort**

Given the array length followed by array values, merge_sort recursively sorts the array in ascending order.
```
$ ./merge_sort 6 6 4 1 2 5 3
C: 1 2 3 4 5 6
Asm: 1 2 3 4 5 6
```
## Given
The starter repo contains C implementations for each of the programs

## Rubric
1. 80 points: automated test cases
1. 5 points: clean repo (no build products) which compiles and links successfully
1. 15 points: code quality: consistent formatting, no dead or redundant code, no unnecessarily complex code, readable comments
1. Fixes may be submitted up to one week after grades are posted
    - Deductions for code quality and clean repo may be fixed for 100% credit
    - Deductions for correctness or incomplete work may be fixed for 50% credit