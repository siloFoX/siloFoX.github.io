---
layout: post
title:  "Computer network 기말 정리"
date:   2020-06-14
excerpt: "발등에 불 떨어진 공대생"
tag:
- computer network
- 컴퓨터 네트워크
- blog
- silofox
comments: true
feature: https://github.com/siloFoX/siloFoX.github.io/blob/master/images/computer-architecture/computer-architecture-feature.jpg?raw=true
---

기본지식 <br>
TCP/IP 에서 레이어는 OSI 와 다르게 5 계층으로 구분한다.
```
Application
Transport : port 로 구분
Network : IP 로 구분
Data link : Physical address 로 구분
Physical
```

A private internet : 다양한 physical network 가 router 에 의해 상호 연결되어 있는 것

<br>

### Comunication at the physical layer

analog -> digital 로 변환하여 전송한다.

>
전송방식
>
유선 : 전기, 가시광선
무선 : 전자기파, 적외선
>

>
전송단위
>
bit
>

physical layer 에서는 한 Data bit 를 node -> node 로 이동시켜준다

<br>

### Communication at the data link layer

하나의 switch 내에서 packet 을 node -> node 로 전달<br>
보내는 단위 : packet (frame 이라고도 함)

<br>

### Communication at the network layer

host-to-host 전달<br>
S -> D 까지 전달하는 것

찾아가는 메커니즘 : routing table 을 이용한(모든 node 에 존재한다.) <b>hop-by-hop 전달</b><br>
한 PN 씩 전달한다.

<br>

### Communication at transport layer

process-to-process 전달<br>
end-to-end 전달

전달단위 : segment(TCP) / userdatagram(UDP)

process 상태(어떤 프로그램이 실제로 돌고 있는 상태)에서 나의 destination 주소를 보고, 웹서버 #port 를 찾아가게된다.

<br>

### Communication at application layer

전송단위 : message

ex) email
```
|송신자 수신자 제목 시간 type delimiter| Text/한글파일/mp3파일|

'/' 는 delimiter (구분자)
```

### 장비

layer 마다 사용하는 장비가 다르다.<br>
L3 : router<br>
L2 : switch<br>
L1 : hub

switch : 각 인터페이스의 host 를 알고 있다. 따라서 Destination 에만 packet 을 전달한다.<br>
hub : 모든 장비로 신호를 다 뿌릴 수 있다. => 회로에는 hub 보다 switch 를 더 많이 사용한다.

<b>L2 만 D, S 순서대로 header 에 작성한다. 나머지는 전부 반대로 작성하므로 주의!!<b>

가장 많이 이용하는 유선랜은 ethernet 무선랜은 wifi 이다.

<br>

### example

웹 페이지에 접속 할 때 <br>
http://home.h.ar.kr 이런식으로 할텐데 http 에서 wks(well known service) 이므로 #port == 80 이다.

그럼 DNS 에 URL 을 질의 하게 되고 응답이 올 것이다. (물론 DNS 서버로 보낼 IP, PORT 도 처리해야 한다)

Client 가 알아야 하는 것은
- 자신의 IP 주소
- DNS 서버 주소
- Router 주소 

집에서 연결하는 경우<br>
laptop/phone (private network) 에서 공유기로 가는데<br>
공유기는 사설망에서 DHCP 서버, Router 역할을 한다.

공유기를 power on 하게 되면
1. IP 주소
2. DNS 서버 주소
3. Router 주소
를 자동으로 받아온다.

* autoconfiguration 설정 => 자동으로 DHCP 로 정보를 가져오게 된다.