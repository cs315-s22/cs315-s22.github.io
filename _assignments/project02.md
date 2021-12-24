---
layout: assignment
due: 2022-02-14 23:59:59 -0800
github_url: https://www.cs.usfca.edu
permalink: assignments/project02.html
---

## Requirements
1. You will develop ARM assembly language implementations of the following problems, and print the results to ensure that both the C implementation and your ARM implementation compute the correct answer.
1. Your executables must be named as follows, and must be compiled with a `Makefile`
1. We will test your projects using `autograder`

**to_upper**
Given a string of upper, lower, and non-alphanumeric characters, transform lowercase letters into uppercase letters.
```
$./to_upper FooBar1
C: FOOBAR1
Asm: FOOBAR1
```
**max3**
Given three signed integer parameters, find the largest by comparing the first two, and then comparing the larger of those with the third. That is, `max3` must be implemented using two calls to `max2`.
```
$ ./max3 2 4 6
C: 6
Asm: 6
```
**find_max_index**
Given an array of integers, return the index of the largest integer in the array
```
$ ./find_max_index 5 4 3 2 1
C: 0
Asm: 0
$./find_max_index 1 2 3 4 5
C: 4
Asm: 4
```
**sort**
Given the address of an array of unsigned integers, and the length of the array, sort the input array descending (largest to smallest) in place given the C implementation of the sorting algorithm. Your sort implementation must use `find_max_index()`.
```
$ ./sort 10 30 20
C: 30 20 10
Asm: 30 20 10
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