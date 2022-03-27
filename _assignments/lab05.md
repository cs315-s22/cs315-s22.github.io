---
layout: assignment
due: 2022-04-24 23:59:59 -0800
github_url: https://classroom.github.com/a/mrY-01HV
permalink: assignments/lab05.html
---

{% assign img_base = site.url | append: site.baseurl | append: "/assets/img" %}

## Background
We are now learning the basics of digital logic and digital circuits with the goal of learning enough to build a working computer processor. We will use the Digital application to build and simulate digital circuits:

[https://github.com/hneemann/Digital](https://github.com/hneemann/Digital)

1. For this lab your are going to build and simulate some simple circuits
1. You can commit your .dig files to github since they're just XML
1. Be careful to name your circuit inputs, outputs, and filenames as shown below

## Part 1: LEDs
You will build a circuit in with two inputs and 4 LED output that shows how 4 basics gates work: NOT, AND, OR, and XOR. The LED will turn red when the input is high (1) and black when in the input is low (0). Here is a screen shot of the circuit you need to build and simulate. Note that you need to add labels to the inputs (A and B):

![lab05-leds]({{ img_base }}/lab05-leds.png)

## Part 2: Max2
Consider the C code fragment below:
```
if (a > b) {
    r = a;
} else {
    r = b;
}
```
1. We will assume that a, b, and r are 2-bit values. That is the only values a, b, and r can be are 0, 1, 2, or 3. 
1. Your job is to use sum-of-products to come up with a Boolean algebra equation and a circuit that will compute the max value r (which is also a 2-bit value). 
1. Your inputs will be a1 a0, b1, b0 and your outputs will be r1 and r0. So, you will have two Boolean equations, one for r1 and one for r0.  
1. Since there are 4 inputs you will have 2^4 (16) rows in your truth table. 
1. Submit a working circuit that correctly produces the max value of the two inputs. You do not need to submit the truth table or Boolean algebra equation.
1. Your circuit **must** have input names a1, a0, b1, and b0. The file **must** be called `max2.dig`

## Part 3: 1-bit full adder
1. In lecture, we built a 1-bit half adder (that is, an adder which does not have a carry-in) and showed the Boolean Algebra equation for a 1-bit full adder. 
1. You will build a 1-bit full adder in Digital, including a CIN and a COUT.
1. Your circuit **must** have inputs named a, b, and cin, and outputs named sum and cout. Your file **must** be named `1-bit-full-adder.dig`
1. Here is the truth table and sum-of-products equations for Sum and COUT 

![lab05-1bfa]({{ img_base }}/lab05-1bfa.png)

## Part 4: 8-bit Ripple Carry Adder
1. You will implement an 8-bit Ripple Carry Adder with two 8-bit inputs, a 1-bit Carry In, one 8-bit result output, and a 1-bit Carry Out.
1. Your circuit **must** have inputs named a, b, and cin, and outputs named sum and cout. Your file **must** be called `8-bit-ripple-carry-adder.dig`

## Rubric
1. 80 pts: LEDs and one of max2 or 1-bit full adder
1. 90 pts: LEDs and both max2 and 1-bit full adder
1. 100 pts: all parts