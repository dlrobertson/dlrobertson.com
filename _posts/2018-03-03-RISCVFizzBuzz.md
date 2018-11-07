---
layout:   post
type:     blog
title:    FizzBuzz Written in RISC-V Assembly
date:     2018-03-03
summary:  A simple super hacky implementation of FizzBuzz in RISC-V Assembly
keywords: fizzbuzz, risc-v, assembly, code, programming
---

I could not find an implementation of [FizzBuzz] written in [RISC-V] Assembly, so
I made a quick attempt at one... Why? I have no idea.

Built with something like the following

```sh
$ uname -r
4.15.1-gentoo
$ riscv64-unknown-elf-as ./fizzbuzz.s -o fizzbuzz.o
$ riscv64-unknown-elf-ld ./fizzbuzz.o -o fizzbuzz
$ spike pk ./fizzbuzz
```

`fizzbuzz.s`

```
.section .rodata
fizz_str:
  .string "Fizz!\x0a"
buzz_str:
  .string "Buzz!\x0a"
fizzbuzz_str:
  .string "FizzBuzz!\x0a"

.section .text
.globl _start

get_ascii:
  li   t0, 10
  blt  a0, t0, lt10
  addi a0, a0, 0x57
  jr   ra
lt10:
  addi a0, a0, 0x30
  jr   ra

printnum:
  addi sp, sp, -32
  sd   ra, 0x18(sp)
  sd   s0, 0x10(sp)
  sd   a0, 0x8(sp)
  addi s0, sp, 32
  andi a0, a0, 0xf0
  sra  a0, a0, 4
  call get_ascii
  sb   a0, 0x0(sp)
  ld   a0, 0x8(sp)
  andi a0, a0, 0x0f
  call get_ascii
  sb   a0, 0x1(sp)
  li   a0, 0x0a
  sb   a0, 0x2(sp)
  mv   a0, sp
  li   a1, 3
  call printstr
  ld   ra, 0x18(sp)
  ld   s0, 0x10(sp)
  addi sp, sp, 32
  ret

printstr:
  mv   a3, zero
  mv   a4, zero
  mv   a5, zero
  mv   a2, a1
  mv   a1, a0
  li   a0, 0
  li   a7, 64
  ecall
  jr   ra

_start:
  li   s2, 0
  li   s3, 101
  j    loop_end
loop:
  li   s4, 15
  rem  s5, s2, s4
  beq  zero, s5, fizzbuzz
  li   s4, 5
  rem  s5, s2, s4
  beq  zero, s5, buzz
  li   s4, 3
  rem  s5, s2, s4
  beq  zero, s5, fizz
print_num:
  mv   a0, s2
  call printnum
  j    loop_end
fizzbuzz:
  la   a0, fizzbuzz_str
  li   a1, 10
  j    print_str
fizz:
  la   a0, fizz_str
  li   a1, 6
  j    print_str
buzz:
  la   a0, buzz_str
  li   a1, 6
print_str:
  call printstr
loop_end:
  addi s2, s2, 1
  blt  s2, s3, loop
  li   a0, 0
exit
  li   a0, 0
  li   a7, 93
  ecall
```

[RISC-V]: https://riscv.org/
[FizzBuzz]: https://en.wikipedia.org/wiki/Fizz_buzz
