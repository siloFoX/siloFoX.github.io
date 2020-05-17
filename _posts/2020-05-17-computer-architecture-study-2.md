---
layout: post
title:  "Computer Architecture study 2"
date:   2020-05-17
excerpt: "하드웨어 ISA 그리고 Microarchitecture"
tag:
- computer architecture
- 컴퓨터 구조
- hardware
- 하드웨어
- trade-off
- 트레이드오프
- ISA
- Microarchitecture
- blog
- silofox
comments: true
feature: https://github.com/siloFoX/siloFoX.github.io/blob/master/images/computer-architecture/computer-architecture-feature.jpg?raw=true
---

목표 : 컴퓨터 시스템의 구성과 그 설계 과정에서 취해지는 트레이드오프에 대한 이해

말이 어렵다 우선 트레이드오프가 뭔지 찾아보기 전에<br>
컴퓨터 시스템의 구성에서 하드웨어를 짚고 넘어가도록 하자.

# 컴퓨터의 하드웨어 

컴퓨터 하드웨어의 구성 요소가 무었일까?
- ALU (산술/논리연산장치)
- Control unit (제어장치)
- Memory (기억장치)
- input (입력장치)
- output (출력장치)

흔히 이렇게 5개로 표현한다.<br>
특히 ALU 와 Control unit 을 합쳐서 CPU(Central Processing Unit) 이라고 한다.

ALU는 논리회로 시간에 일부분 배워서 조금 익숙하고, <br>
메모리는 내부 구조는 잘 모르지만 프로그래머의 관점에서의 기능은 파악하고 있을 것이다.<br>
입력/출력은 따로 설명이 필요없이 직관적이다.

그런데 Control unit 그 역할이 뭔지는 들었지만<br>
(주로 하드웨어를 제어하는 장치..? 정도)<br>
...자세한 공부가 필요하다.

4장의 전반부 (non-piplined)를 보면 <b>datapath</b> 파트와 <b>control</b> 파트로 분리되어있다.<br>
<b>ALU 는 <b>datapath</b> 에 포함되고 <b>control</b> 그 뒤를 따른다.</b><br>
이 부분은 간단한 프로세서(<b>datapath + control</b>)의 연결 형태가 구체화하는 과정이라고 한다.<br>
후반부 (piplined)에서도 동일하게 단계적으로 소개될 것이다.

<br>

# 트레이드오프?

컴퓨터구조에서는 다양한 상충 문제가 발생한다.<br>
기술, 가격, 속도, 전력소모, 신뢰성, testability 등등<br>
다양한 제한과 요구조건을 동시에 만족시키기는 어렵다.

>
trade-off 를 이해해보자.
>
trade-off 는 다양한 의미로 사용되고 있어서, 국문의 번역도 획일적이지 않다고 한다.<br>
취사선택이라는 말이 가장 많이 쓰인다고는 하는데<br>
컴퓨터 구조에서의 의미에서는 중요도가 높은 선택사항을 취하기 위해서<br>
어느정도 중요도가 낮은 사항을 희생시키는 제로섬 같은 형태를 말하는 것 같다.
>

<br>

# ISA 와 Microarchitecture

ISA 는 hardware/software interface 라고 첨언이 되어있다.<br>
명령어 집합 구조는  하드웨어/소프트웨어간의 상호작용을 하는 곳<br>
이라는 뜻이다.

그리고 ISA 는 그 상호작용을 하는 곳을 추상화시킨 것이다.<br>
그 아래에는 microarchitecture (computer organization 이라고도 한다.)가 있다.
이 부분이 기계어를 이해하고 실행할 수 있는 부분이다. <br>
그리고 이것들 모두 컴퓨터구조에 포함된다.

>
말 그대로 소프드웨어에서 하드웨어의 중재자 역할이라고 할 수 있다.<br>
ISA 는 프로그래밍 인터페이스 중에서 최하위 레벨에 속한다.
>
그리고 ISA 를 물리적으로 구현하는 방법을 마이크로아키텍쳐 혹은 컴퓨터 조직이라고 한다.<br>
그리고 같은 명령어 집합 구조를 서로 다른 마이크로아키텍쳐로 구현하기도 한다.
>

<br>

추상화는 논리회로설계에서도 볼 수 있다.<br>
논리회로 설계에서 AND, OR, NOT 등의 게이트들을 symbol 로 표시하고<br>
이것들을 엮어서 원하는 논리함수를 구현했다.

실제로 그런 symbol 은 현실에 존재하는 것은 아니고,<br> 
내부는 transistor 들로 구성되어 있지만<br>
그 내부를 몰라도 설계를 할 수 있는 것이다. 그리고 이것이 추상화의 예이다.

<br>

아주 간단한 프로세서를 만든다고 해보자.

명령어는 8개로 (add, nor, load, store, beq, branch, equal, 등등) 하자.<br>
길이는 32비트로 하기로 하고, <b>register file</b> 의 크기는 16으로 정한다.

생각해봐야 할 것은
- 명령어를 32bit 로 표현해야 되는데 어떻게 표현해야 할까.
- 피연산자의 주소는 어떻게 표현할까

이런식으로 ISA 가 정의되고 나면, 어셈블리어/기계어 프로그래머는<br>
이를 기반으로 프로그램을 작성할 수 있다.<br>
내부가 어떻게 구성되어서 각 명령어가 실행되는지는 상관없이 말이다.