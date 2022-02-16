---
layout: assignment
due: 2022-02-21 23:59:59 -0800
github_url: https://classroom.github.com/a/Zkv2W9kC
permalink: assignments/lab03.html
---

## Requirements
1. You will develop ARM assembly language implementations of the following recursive problems. Your solution must be recursive. No credit will be given for an iterative solution. 
1. Your executable must be compiled with a `Makefile`
Before you add files to your repo, do a `$ make clean` so you don't add/commit build products like executables or .o files
1. We will test the labs using `autograder`

**fibrec**: Recursive Fibonacci number generator. Example:
```
$ ./fibrec 10
C: 55
Asm: 55
$ ./fib_rec 20
C: 6765
Asm: 6765
```
**sumarr_rec**: Recursive sum array. Example:
```
$ ./sumarr_rec 1 2 3 4 5
C: 15
Asm: 15
./sumarr_rec -1 0 -3 0 -9 0 9 0 3 1
C: 0
Asm: 0
```
**is_pal_rec**: Recursive palindrome checker. Example:
```
$ ./is_pal_rec abba
C: True
Asm: True
$ ./is_pal_rec racecar
C: True
Asm: True
$ ./is_pal_rec  cs315sc
C: False
Asm: False
$ ./is_pal_rec x
C: True
Asm: True
```

## Given
1. We will demonstrate a framework for compiling C and assembly language source files, and calling assembly language functions from C. 
1. We provide C implementation of `fibrec_c()`, `sumarr_rec_c()`, `is_pal_rec_c()`.

## Rubric
1. For 80 pts, your implementation will pass the test cases for one of the three problems
1. For 90 pts, your implementation will pass the test cases for two of the three problems
1. For 100 pts, your implementation will pass all of the test cases