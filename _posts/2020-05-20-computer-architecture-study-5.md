---
layout: post
title:  "Computer Architecture study 5"
date:   2020-05-20
excerpt: "DRAM 과 SRAM 그리고 메모리 구조"
tag:
- computer architecture
- 컴퓨터 구조
- DRAM
- SRAM
- memory
- row buffer
- blog
- silofox
comments: true
lastmod : 2020-05-20
sitemap : 
  changefreq : never
  priority : 1.0
feature: https://github.com/siloFoX/siloFoX.github.io/blob/master/images/computer-architecture/computer-architecture-feature.jpg?raw=true
---

2020-03-26 Lecture 2-2

# DRAM

현재 많은 컴퓨팅 시스템에서 <b>DRAM (Dynamic Random Access Memory) </b>를 쓴다.<br>
고집적이 가능하고 용량대비 가격이 상대적으로 낮은 편이다. <br>
그러나 DRAM 은 저장된 정보가 시간이 지나면 상실된다.<br>
그래서 주기적으로 <b>refresh(read and rewritten 을 뜻한다.)</b> 시켜야 하는데<br>
이것을 <b>dynamic</b> 이라고 한다.

또 전원이 공급되지 않으면 저장된 정보는 상실되므로 휘발성(volatile) 메모리에 속한다.<br>
오늘 얘기는 이 메모리에 대한 얘기이다.

그 전에 tri-state buffer 에 대한 지식이 없다면 한번 검색하는 것을 추천한다.

# 메모리 구조

16x1 memory 를 예로 들어 이해해보자.<br>
16개의 location 에 각각 1 bit 의 데이터가 저장되어 있다.<br>
각 cell 의 내부 구성은 transistor 와 capacitor 를 모르는 경우에 불편하기 때문에 생략한다.<br>
일렬로 늘어선게 아니고 rectangular array 형태이다.<br>
(16x1 의 메모리가 4x4 형태로 되어있는 것이다.)<br>
즉 메모리가 행과 열의 형태로 구성되어서 행과 열의 교차점에 1 bit 의 정보가 저장된 것이다.<br>
Manhattan-like street layout 이라고도 한다.<br>

16개의 location을 분리하려면 address 는 몇 비트로 해야할까?<br>
당연히 4 bit 이다.<br>

이것들을 A0A1 과 A2A3 의 두 비트씩으로 나눈 것을 decoder 에 입력하는 것이다.<br>
정사각형 형태이므로 A0A1 두 비트가 decoding 되어서 행을 선택하고<br>
A2A3 는 열을 선택하는 형태로 이뤄질 것이다.<br>
마치 뽑기기계의 x, y 축을 이동하는 것 같이 말이다.

<br>

이번에는 64x1 을 예시로 보자.<br>
2^6 = 64 이므로 <br>
6 bit address 로 접근하면 되겠다.<br>

프로세서와 DRAM 사이에는 <b>memory controller</b> 가 존재하여서<br>
상호연락을 담당한다. (편의상 간단하게 표현한 것이다.) 

프로세서는 request 를 memory controller 에게 보낸다.<br>
request 가 memory controller 에 도착하면 queue 에 저장되어 있다가,<br>
스케쥴링 방식에 따라서 <b>command sequence</b> (memory controller 와 DRAM 사이의 protocol)<br>
형태로 DRAM 에 보내진다.

<b>Processor <-> memory controller <-> DRAM</b>

<br>

CPU 가 보낸 address의 형태는 뭘까?<br>
010110 이라면 010 과 110dmfh 나뉘어 전달된다.<br>
그것은 <b>row address</b> 와 <b>column address</b> 로 나뉘어서 순차적으로 DRAM 으로 보내진다.<br>
(순차적 : time-multiplexed 를 말하는 것이다.)

그런데 또 부가적으로 1 bit 를 더 사용한다.<br>
DRAM 내부에 <b>row buffer (sense amplifier)</b> 라는 것이 존재한다.<br>
그것은 또 뭘까??

address <b>010110</b> 은 row 와 column 으로 나뉘어서 row address 에 <b>010</b> 이 먼저 보내지고<br>
010 이므로 3번째 row 전체가 읽혀서 <b>row buffer</b> 에 저장된다.<br>

그 다음 column address 로 <b>110</b> 이 보내지면,<br>
110 은 7이므로 7번째 column 에 해당하는 데이터가 선택되어서 출력되게 된다.

또 질문들 왜 하나의 주소를 굳이 나눠 공급하는 것이며, Row buffer 는 무엇일까??

<br>

DRAM chip 과의 통신은 chip 의 pin 을 통해서 이뤄진다.<br>
DRAM chip 설계에서 지금까지도 중요한 고려사항 중 하나가 pin 의 수를 줄이는 것이다.<br>
row/column 으로 address 를 나눠서 순차적으로 보내면 필요한 pin 의 수가 줄어들게 되는 것이다.<br>
(pin 은 데이터가 나가는 통로는 말하는 듯하다.)

<br>

그 다음은 row buffer 위에서 설명한대로 해당 row 의 모든 데이터는 row buffer 에 실린다.<br>
그리고 read/write 동작은 이 row buffer 를 통해서 수행된다.

row buffer 는 임시 데이터 저장소로 작용되어서 DRAM 메모리 시스템의 성능 향상을 위해서 사용된다.<br>
정확한 이해를 위해서 DRAM 의 데이터를 access 하는 과정을 단계적으로 설명해야 하므로 생략한다.

메모리 request 들이 memory controller 의 queue 에 저장된다고 했다.<br>
request 들을 요청 순서대로 처리하는 경우에 그 때 마다 새로운 row/column 을 거쳐서 접근하게 된다.

그러나 row buffer 에 임시 저장된 data 를 사용할 수 있다면 성능 개선의 효과가 있을 것이라 예상할 수 있다.<br>
그리고 DRAM controller 는 자신에게 들어오는 request 들을 순서대로 처리하지 않고,<br>
자체적으로 스케쥴링을 통해여 효율적으로 처리할 수도 있을 것이다.<br>
이를 <b>row buffer management policy</b> 라고 한다.

<br>

# SRAM

마지막으로 SRAM(static RAM) 에 대해 간단히 소개한다.<br>
DRAM 과 SRAM 은 cell 의 구성자체가 다르다.

단위 bit 당 가격은 SRAM 이 높지만, 상대적으로 빠르게 access 할 수 있는 메모리이다.<br>
첫번째 시간에 메모리계층구조에서 언급한 cache memory 에 사용된다. <br>
cache 는 프로세서와 느린 main memory  사이에서 속도차를 극복하기 위한 프로그래머에게는 숨겨진 메모리이다.

<br>

DRAM 은 기업이 주도하는 분야이므로 정보가 많이 공개되지는 않는 것 같다.<br>
그래서 오늘 내용이나 소개 방식에 오류가 있을 수 있다.

다음은 MIPS 명령어에 대한 학습을 시작한다.