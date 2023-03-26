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
lastmod : 2020-05-21
sitemap : 
  changefreq : never
  priority : 1.0
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

Register 들을 정의하고, 어떤 명령어들로 명령어 집합을 구성하고, <br>
이것들을 어떻게 binary 로 표현하는가(<b>how to encode instructions</b>) 등등의 정의이다.

명령어들이 모두 32 bits 로 표현된다고 하자.<br>
그러면 어떻게 이 32 bits 를 여러 필드로 나눠서 명령어들을 binary 로 표현할 것인가 하는 문제이다.<br>
명령어는 기본적으로 <b>opcode</b>(operation code) 와 피연산자를 찾기 위한 정보를 포함해야 하니까 말이다.<br>
instruction format 이 정해지면 microarchitecture 담당자가 이에 따라 해석 및 처리하는 하드웨어를 구성 할 것이다.

<br>

## CISC 와 RISC 에 대한 비교

우선 명령어의 수에 있어서 현저한 차이가 난다.<br>
SPARC 은 60개 정도인데 CISC 는 몇 백개이다.<br>

CISC 의 경우에는 명령어의 길이도 다양하여 IA-32 의 경우 1~17 bytes 이다.<br>
피연산자의 location 을 찾는 과정(<b>addressing modes</b>)도 상대적으로 복잡하다.<br>
CISC 의 명령어를 해석하려면 RISC 명령어보다 훨씬 복잡할 것이다.<br>
우리가 해석하기 힘들다는 것은 제어장치(control)도 복잡하다는 의미이다.

<br>

## 간단한 프로세서의 동작

CPU 내부에는 register 들이 있다. <br>
일반적인 register 들과 특수 목적의 register 들이 있을 것이다.<br>
내부의 register 들은 빠른 속도로 접근이 가능하다.

<b>Harvard Architecture</b> 와 흔히 들었던 <b>von Neumann architecture (Princeton Architecture)</b> 의 차이점은<br>
instruction memory 와 data memory 의 분리이다.

Havard Architecture 는 instruction memory 와 data memory 가 분리되어서 각각 별도의 bus 로 프로세서와 연결된 구조이다.

<br>

명령어가 실행되는 과정을 3단계로 분리하여 설명한다.<br>
명령어를 메모리로부터 가져오는 <b>fetch</b><br>
명령어를 해석하는 <b>decode</b><br>
실행하는 <b>execute</b><br>
로 표현하고 있다.

시작은 <b>PC(Program Counter)</b> 이다.
