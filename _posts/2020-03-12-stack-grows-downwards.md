---
layout: post
title: "Stack Grows Downwards"
categories: reverse-engineering stack
---

*What your instructor means when she says stack grows downwards 
(in an Operating Systems (OS) or Computer System Organizations (CSO) class).*

Disclaimer: The following post is written to build visual intuition 
on a little-endian system. For someone working on a big-endian system,
a different visualization will be more helpful.

In terms of the sheer address, this is how she thinks the stack looks like.

FF  |  FE  |  FD  | FC | \<higher address>
FB  |  FA  |  F9  | F8
F7  |  F6  |  F5  | F4 | 
F3  |  F2  |  F1  | F0 | `esp` ← F0
EF  |  EE  |  ED  | EC |
..  |  ..  |  ..  | ..
..  |  ..  |  ..  | ..
07  |  06  |  05  | 04
03  |  02  |  01  | 00 | \<lower address>

Let's assume a little endian system with following values:  
**0xFC:** `0xA0A1A2A3`  
**0xF8:** `0xA4A5A6A7`  
**0xF4:** `0xA8A9AAAB`  
**0xF0:** `0xACADAEAF`  

The exact same layout but now with values looks like this:

 φ + 3 | φ + 2  |  φ + 1 |  φ + 0 | ADDRESS φ
|:----:|:------:|:------:|:------:|:-------:
  A0   |   A1   |   A2   |   A3   | **FC**
  A4   |   A5   |   A6   |   A7   | **F8**
  A8   |   A9   |   AA   |   AB   | **F4**
  AC   |   AD   |   AE   |   AF   | `esp` ← **F0**

The following is what happens when a new value say `0x11223344`
is pushed to the stack.

 φ + 3 | φ + 2  |  φ + 1 |  φ + 0 | ADDRESS φ
|:----:|:------:|:------:|:------:|:-------:
  A0   |   A1   |   A2   |   A3   | **FC**
  A4   |   A5   |   A6   |   A7   | **F8**
  A8   |   A9   |   AA   |   AB   | **F4**
  AC   |   AD   |   AE   |   AF   | **F0**
  11   |   22   |   33   |   44   | `esp` ← **EC**

Note that the value of ESP decreased and
the stack grew a tiny bit `downwards`. This is what is happening in theory.

In practice following is how a debugger like gdb would diplay the output.

```
EC: 11223344 
F0: A4A5A6A7
F4: A8A9AAAB
F8: ACADAEAF
FC: A0A1A2A3
```

At the same time, if `scanf` was taking 20 bytes as string and writing it to
the stack start at address EC, the input provided should look as follows:

`44332211` `A7A6A5A4` `ABAAA9A8` `AFAEADAC` `A3A2A1A0`

in order to make the final stack state look as the one above.

