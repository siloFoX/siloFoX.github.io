---
layout: post
title:  "Computer Architecture study 6"
date:   2020-05-21
excerpt: ""
tag:
- computer architecture
- 컴퓨터 구조
- blog
- silofox
comments: true
feature: https://github.com/siloFoX/siloFoX.github.io/blob/master/images/computer-architecture/computer-architecture-feature.jpg?raw=true
---

2020-03-27 Lecture 3

# MIPS ISA

ISA 는 hardware 와 low-level software 간의 interface 라고 했었다.<br>
그리고 ISA 는 컴퓨터 구조의 일부이고.

RISC 가 등장하기 전, 전통적으로 프로세서는 complex instruction set computer 방향으로 발전되었었다.<br>
그러나 1980 년도 초에 RISC 기반의 microprocessor 가 출현한 것은 ISA 의 커다란 변화였다.<br>
ISA 가 정의되면 이것을 기반으로 하드웨어를 구현하게 되므로 ISA 의 정의는 성능과 연관된다.<br>

## ISA 에 관한 간단한 정리

- Defines registers
- Defines data transfer modes (instruction) between registers, memory and I/O
- There should be sufficient instructions to efficiently translate any program for machine processing

- Next, define instruction set format - binary representation used by the hardware<br>
Variable-length instructions vs. fixed-length instructions

