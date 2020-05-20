---
layout: post
title:  "Computer Architecture study 1"
date:   2020-05-16
excerpt: "컴퓨터 구조 개론"
tag:
- computer architecture
- 컴퓨터 구조
- RISC
- CISC
- ISA
- blog
- silofox
comments: true
feature: https://github.com/siloFoX/siloFoX.github.io/blob/master/images/computer-architecture/computer-architecture-feature.jpg?raw=true
---

컴퓨터 구조에 대한 강의를 들으면서 나름대로 정리한 내용이며, 정보의 정리글이 아닙니다.<br>
편안한 마음으로 공부해보도록 합시다.<br>
강의 내용과 인터넷에서 찾아서 정리한 내용으로 구성됩니다.<br>

교재는 Computer Organization and Design The Hardwar/Software Interface Fifth Edition<br>
(David A. Patterson, John L. Hennessy) 입니다.

2020-03-18 Lecture 1-1

<br>

# 개론

컴퓨터구조란 무엇일까?
컴퓨터의 내부 구성일까?

<br>

>
위키에 따르면 컴퓨터 구조는 명령어 집합 구조(ISA), 마이크로아키텍쳐 설계 및 구현을 의미한다.<br>
운영체제 및 응용 프로그램 - 커널 - 어셈블러 - 펌웨어 - 하드웨어 
>

<br>

1980년대 초에 미국에서 집적회로 기술의 눈부신 발전이 있었다.<br>
그 때 컴퓨터 구조와 관련된 연구가 매우 활발했던 것 같다.

당시의 관점에서 RISC 는 단일 프로세서의 성능 향상을 추구했다면,<br>
CISC 는 단일 프로세서를 여러 개 사용하여 병렬 처리를 통한 성능 향상을 도모한 것으로 볼 수 있다.

x86 프로세서는 CISC 로 분류된다. 그런데 과거와 달리 x86프로세서도 명령어들을 <br>
내부적으로 더 작은 단위로 나눠서 실행하여서 내부에서 RISC 처럼 동작한다고 보기도 한다.<br>

오래 전에 CISC와 RISC의 우열에 대한 논쟁이 있었는데, 나중에 차이점을 언급하도록 하겠다.

C 언어와 달리 어셈블리는 프로세서가 의존적이다.<br>
SPARC 과 MIPS 모두 RISC 구조지만 명령어 집합 구조(Instruction Set Architecture)가 다르다고 한다.<br>
ISA 는 컴퓨터 구조의 한 부분이다.

<br>

>
나중에 배울테지만 미리 찾아본다.
>
RISC 와 CISC
>
RISC (Reduced Instruction Set Computer, 축소 명령어 세트 컴퓨터)<br>
명령어의 개수가 적은 컴퓨터라는 뜻인 것 같다. 파이프라이닝이 가능하다.(중요한 것 같다.)<br>
빠른 동작속도와 하드웨어의 단순화로 효율이 좋고, 가격도 싸다.
>
CISC (Complex Instruction Set Computer, 복잡 명령어 세트 컴퓨터)<br>
연산을 처리하는 복잡한 명령어들을 수백개 이상 탑재하고 있는 컴퓨터이다.<br>
명령어마다 길이가 다르고, 실행에 필요한 사이클 수도 달라서 파이프라인 설계가 어렵다.<br>
그러나 많은 프로세서가 CISC 모델로 구축되어 있고, 호환성이 좋다.
>
[출처 : Jake.lee's Blog : 컴퓨터 구조 : RISC 와 CISC](https://frontalnh.github.io/2018/04/17/%EC%BB%B4%ED%93%A8%ED%84%B0-%EA%B5%AC%EC%A1%B0-risc-%EC%99%80-cisc-%EA%B5%AC%EC%A1%B0/)
>

<br>

# 교재

1장은 전반적인 얘기이다. 1.6(성능) 1.7(전력 장벽) 1.10(오류 및 함정) 를 읽어보는게 좋다.<br>
특히 성능 부분은 전체적으로 계속 사용되니까 읽어보자.

2장, 3장은 4장을 위한 준비과정이다. 간단한 프로세서의 ISA도 소개해준다.<br>
어셈블리가 처음이라면 읽어보기를 권한다. 

4장은 전반부와 후반부로 나뉜다.<br>
전반부는 MIPS 명령어와 32-bit ALU, 논리회로 지식을 기반으로 간단한 프로세서 구현을 배운다.(<b>non-piplined</b>)<br>
후반부에는 성능 향상을 위해서 piplined 구조로 변경한다. 전반부와 후반부 사이의 성능적인 측면에서 비교하게 된다. <br>
핵심은 후반부의 파이프라인 구현이다.

5장에서는 메모리계층구조를 소개한다.<br>
위 과정에서 메모리를 하나의 단순한 블록으로 간주한다.<br>
프로세서의 관점에서 address 를 메모리로 보내 해당 address 에 저장된 데이터를 받는 것이 읽기이다.<br>

여기서 메모리는 컴퓨터의 성능을 위해 매우 중요한 부분이다.<br>
프로세서가 아무리 빨라도 데이터가 적절한 순간에 공급되지 못 하면 의미가 없기 때문이다.<br>
그래서 우리는 속도도 빠르고 용량이 크고 저렴한 메모리를 원한다.<br>
그런데 현실적으로 상충되는 요구이다.

그래서 속도가 빠르고 용량이 큰 메모리가 있는 것 같은 효과를 가지는 메모리계층구조(cache - main memory - disk)를 소개 한다.<br>
그리고 cache 설계(cache - main memory 계층)과 virtual memory(main memory - disk 계층)에 대해서도 다룬다.<br>
(Virtual memory는 운영체제와 일부 겹치는 부분이므로 잘 학습해보자.)