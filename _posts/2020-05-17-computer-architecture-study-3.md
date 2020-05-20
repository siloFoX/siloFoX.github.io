---
layout: post
title:  "Computer Architecture study 3"
date:   2020-05-17
excerpt: ""
tag:
- computer architecture
- 컴퓨터 구조
- Processor
- 프로세서
- piplined
- 파이프라인
- Superscalar
- 슈퍼스칼라
- Out-of-order
- blog
- silofox
comments: true
feature: https://github.com/siloFoX/siloFoX.github.io/blob/master/images/computer-architecture/computer-architecture-feature.jpg?raw=true
---

2020-03-20 Lecture 1-3

# 프로세서의 변천사

프로세서의 변천사를 살펴보도록 하자.<br>
transistor count 와 clock frequency 의 변화도 같이 비교해 보자.

Intel 4004 는 2,250 transisors 와 108 KHz 이다. 매우 답답했을 것 같다.<br>
8086 은 29,000 transistors 에 5 MHz <br>
80486 은 1st <b>piplined</b> inplementation of IA32 이다.<br>
IA32 란 Intel 의 32 bit Architecture 라는 뜻이다.

여기서 piplined 란 무슨 뜻 일까?

<br>

# piplined

명령어라는 표현은 여기저기에 사용된다. <br>
이 수업에서 명령어란 ISA 에 정의된 instruction 들이다.<br>

[study 2](https://silofox.github.io//computer-architecture-study-2/)에서 정의된 것이라면, 8개로 정해졌을 것이다.<br>
이 명령어들은 제 기능을 수행하도록 내부를 구성하는 것이 프로세서의 구현이다.<br>
물론 성능적인 측면은 고려해야한다.

ISA 를 가지고 프로세서를 구현해야 한다고 생각해보자.<br>
team 1 은
```
    1. 명령어 하나를 메모리에서 가져와서
    2. 해석해서
    3. 실행하고
    4. 저장한다.
    5. 위 과정을 반복한다.
```
이것을 non-piplined 설계라고 한다.

team 2 는 명령어를 가져오고, 해석하고, 실행하고, 저장하는 과정을<br>
몇 단계로 분리하여서<br>
첫 명령어를 가져오고 해석할 때<br>
동시에 그 다음 명령어를 가져오고<br>
그 다음에는<br>
처음 명령어가 실행하는 단계일 때 두번째는 해석, 세번째는 가져오기<br>
뭐 그런 식으로 설계를 했는데<br>
그것을 piplined 설계라고 한다.<br>
4장에서 중점적으로 다루게 된다.

team 1 과 team 2가 설계한 프로세서는 architecture 가 다를까?<br>
여기서는 두 프로세서의 architecture 는 같은데<br>
구현이 다르다고 표현한다.

둘 다 동일한 기계어 프로그램을 문제없이 실행한다.<br>
하지만 구현이 달라서 성능의 차이가 나는 것이다.

<br>

# 다시 프로세서

다시 프로세서 얘기로 돌아가자<br>

Pentinum 

3,100,000 transistors 와 60 MHz<br>
1st Superscalar implementation of IA32<br>
Superscalar 라는 것은 처음 본다.<br>

486에서 Pentium 으로의 변화는 piplined 에서 <b>Superscalar</b> 로 넘어간 것이라고 한다.<br>
왜 그럴까?<br>
하나의 pipline 으로는 성능상의 병목이 있어서 그렇다고 한다.<br>

프로그램은 반드시 주어진 순서대로 하나씩 실행해야 하는 것은 아니다.<br>
예를 들어서<br>
(r 은 register)
```
1   r2 <- r1 + r3
2   r4 <- r5 + r6
```
에서 1 2 순서대로 하든 안 하든 결과는 같다. (순서의 의미가 없다는 뜻이다.)<br>
프로그램상의 순서는 있지만 말이다.

Pentium은 2개의 파이프(u-pipe, v-pipe 였던 것 같다고 하셨다.)로 구성된 것으로 볼 수 있다.<br>
그리고 두 명령어의 독립성이 만족되는 경우에 한하여 동시에 실행될 수 있게 하였다.<br>

앞 명령어의 결과를 다음 명령어가 피연산자로 사용하는 경우는 당연히 제외된다.<br>
명령어들의 실행순서는 프로그램의 순서를 준수하도록 설계되었다.<br>

<br>

Pentium 4

55,000,000 transistors 에 3GHz <br>
Out-of-order execution<br>
(not the first)

트랜지스터와 clock frequency 의 현저한 변화가 눈에 띈다.<br>
그런데 <b>Out-of-order execution</b> (dynamic execution) 은 또 뭘까?<br>
예시를 보자.
```
1   r1 <- r4 / r7     // 나누기는 4 cycle 소요된다고 가정한다.
2   r8 <- r1 + r2
3   r5 <- r5 + r9
4   r6 <- r6 - r3
5   r4 <- r5 + r6
6   r7 <- r8 * r4
```
명령어 1과 2, 2와 6, 3과 5, 4와 5, 5와 6 사이는 의존적이다.<br>
1의 결과가 2에서 피연산자로 사용되기 때문에 그렇다.

In-order execution 이라면 그냥 순서대로 실행된다.<br>
In-order 이면서 2-way Superscalar 라면 종속성이 없는 경우에 두 개가 동시에 실행된다.<br>
Out-of-order execution 의 경우에는,<br>
<b>피연산자</b>와 <b>실행장치</b>(ALU 같은)의 개수가 <b>avilable</b>(충분)하다면 프로그램의 순서에 관계없이 실행이 가능하다.<br>
(1이 실행되는 동안 2는 결과를 기다리더라도 3은 실행이 가능하도록 설계되었다는 뜻이다.)

<br>

그 다음은 Multicore 로 변화된 프로세서들이다.<br>
우리는 현재 Multicore 세대이다.<br>
아마 처음 사용한 컴퓨터도 Multicore 일 것이라고 짐작된다.<br>

Multicore 는 Chip Multicore 라고도 한다.<br>
이전 세대에서 Superscalar 로 지속적인 성능 향상을 하다가, 기술적인 장벽에 부딪혔다.<br>
(<b>instruction-level parallelism</b>)<br>
그래서 한 차원 위의 <b>thread-level parallelism </b>을 추구하게 된 것이다.

<br>

Power 와 성능사이에 비례관계에 대해서..

Superscalar 로 성능을 2배 향상 시키려면 2^1.75 에 비례하는 Power 가 소모된다.<br>
그래서 차라리 성능이 낮고 전력 소모가 적은 core 를 여러개를 사용한다면, <br>
동일한 성능을 조금 더 감소된 전력으로 실현할 수 있다.<br>

<br>

# 첨언

Pipelined, Superscalar, Out-of-order 가 서로 배타적인 것은 아니다.<br>
Superscalar 도 파이프라인이고, 또 대부분은 Out-of-order 이기때문이다.<br>

또한 Out-of-order 이면서 Superscalar 가 아닐 수도 있다.
혼란스럽다면 지금은 무시해도 좋다.