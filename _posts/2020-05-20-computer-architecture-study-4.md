---
layout: post
title:  "Computer Architecture study 4"
date:   2020-05-20
excerpt: "논리회로 복습 MUX 그리고 Register file 과 Clock"
tag:
- computer architecture
- 컴퓨터 구조
- 조합회로
- MUX
- Decoder
- Register file
- Clock
- blog
- silofox
comments: true
feature: https://github.com/siloFoX/siloFoX.github.io/blob/master/images/computer-architecture/computer-architecture-feature.jpg?raw=true
---

2020-03-25 Lecture 2-1

# 논리회로 복습

## 조합회로

<b>Instruction Memory, Data Memory, ALU, Adder, Registers 그리고 MUX (multiplexer)</b><br>
앞에 4개는 그냥 그런가보다 하면 될 것 같은데 뒤에 2개는 좀 명확하게 짚어야 할 것 같다.

2-to-1 MUX 는 S의 값이 0이면 A를 S 값이 1이면 B를 출력하는 블록이다.<br>
A와 B는 각각 독립적으로 0이나 1일 수 있는 값이다.<br>
한 마디로 Data selector 라고 할 수 있겠다.

MUX 는 왜 필요할까?

두 개의 32-bit register 인 A, B<br>
그리고 32-bit adder 가 있다고 가정해보자.

어떤 경우에는 A+4 를 다른 경우에는 B+4 를 수행해야한다면 어떻게 구성해야 할까?

Adder 는 두 개의 32-bit 입력을 받아서 32-bit 의 출력을 만드는 기능을 수행하는 조합회로이다.<br>
그러므로 Adder 의 하나의 입력에 4를 넣고,<br>
다른 곳에는 A 와 B 를 MUX 로 연결하고 S 값으로 둘 중에 하나를 선택하고<br>
MUX 의 출력값을 Adder 에 연결하면 되는 것이다.

<br>

교재의 4.11 에서 ALU 앞에있는 MUX 가 이것과 유사하다.<br>
<b>ALUSrc</b> 가 S의 기능과 같은 것이다.

MUX 는 회로의 공유가 가능하도록 한다.<br>
Bus 구성에 사용된다.<br>
당연히 동시에 사용하는 것은 불가능하다.

Time multiplexing 이라면 스위치를 통해서 시간적으로 교환되는 것이 가능하다.<br>

ALUSrc 는 제어장치에서(Control 에서) 생성하며 datapath 에 공급하는 것으로<br>
나중에 Control 설계시 출력 중에 하나가 된다.

<b>Decoder</b> 도 중요하다.

Decoder 는 n 개의 input 과 2^n 개의 output 을 가지는 회로이다.<br>
입력이 A1A0 = 10 이라면 나오는 결과는 0100 이 될 것이다.<br>

어떤 부분에서 필요한 것인지는 이미 알고있을 것이다.
컴퓨터 구조에서 decoder 가 필요한 구성요소는 무었일까?
바로 메모리이다. 

<br>

## 순차회로

드디어 기억소자가 등장한다.<br>
중요한 것 2가지만 기억하면 된다.

바로 <b>Register file</b> 과 <b>Clocking</b> 이다.

Register file 은 array of registers 라고 볼 수 있다.<br>
당연히 address decoder 를 내장하고 있다.<br>
그 decoder 에 한 register 를 선택할 수 있게 비트를 넣어주면(Data in 으로 들어가게 된다.)<br>
Data out 에 그 register 값에 해당하는 bit(register 가 2-bit 라든지)들이 출력되게 된다.<br>

물론 실제로는 이렇게 단순하진 않고,<br>
register 두개를 동시에 읽으면서 1개의 register 에 데이터 저장이 가능한 형태를 가진다.<br>
추후에 자세한 설명이 나온다.

<br>

수업에서는 <b>edge-triggered clocking 을 사용한다.</b><br>
register 의 상태변화가 clock 이 0 -> 1 로 변하는 그 순간에 발생한다.<br>
그리고 그 이후에는 다음 0 -> 1 로 변하는 순간까지는 값을 유지한다.

<b>Write control</b> 이 추가되면 어떨까?

다른 건 다 똑같지만 값을 저장하는 순간에는 Write control 이 1인 경우로 제한된다.<br>

교재에서 <b>PC(Program Counter)</b>라는 register 를 예시로 비교해보자.<br>
그림 4.19 에는 위에서 내려오는 선이 없지만 4.60에는 <b>PCWrite</b> 라는 제어선이 있다.<br>
이는 4.19 에서는 clock cycle 를 계속 하지만, 4.60 에서는 PCWrite 가 1이 아니면 그 상태를 유지한다.

4장의 그림에서 제어신호가 명시되어 있지 않은 register 는 매 clock cycle 마다 쓰기가 실행된다고 가정하면 될 것 같다.<br>
(그나저나 Hazard detection unit 이라 설마 piplined 에서 Hazard?)

Clock 의 주기는 longest delay 로 결정된다.<br>
예를 들어 register 에서 피연산자를 받아서, adder 에서 더한 뒤에(10^-6s 소요된다는 가정)<br>
그 결과를 register 에 저장하면 1MHz 이상의 clock 은 사용할 수 없다.<br>

계산결과가 나오기 전에 clock edge 를 만난다면 잘못된 값이 저장될 테니까 말이다.<br>
다시 말해서 소요시간보다 빠른 주기를 가지는 clock 은 사용할 수 없다는 것이다.

다음 시간에는 DRAM 에 대해 간단히 소개해보도록 하겠다.