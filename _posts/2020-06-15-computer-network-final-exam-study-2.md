---
layout: post
title:  "Computer network 기말 정리 2"
date:   2020-06-15
excerpt: "발등에 불 떨어진 공대생"
tag:
- computer network
- 컴퓨터 네트워크
- blog
- silofox
comments: true
feature: https://github.com/siloFoX/siloFoX.github.io/blob/master/images/computer-architecture/computer-architecture-feature.jpg?raw=true
---

PN 은 외부와 연결하기 위한 Router 가 필요하다.<br>
PN => partially <b>connected network</b> 로 구성되어 있다.

### Service provided at the source computer

Source Host 가 하는 일
1. header 를 붙여줌
2. N.H.A 의 논리주소 -> 물리주소
3. fragmentation

순서대로 기술하자면,<br>
Layer 4 packet 이 들어오면 먼저 packetize 를 한다.<br>
next-hop logical address 를 Routing table 통해 찾는다.<br>
next-hop MAC address 를 ARP 를 통해서 찾는다.<br>
MTU(Maximum Transfer Unit) table 을 통해서 Fragemtation

<br>

### Control

* 오류제어 
L3H 에서는 Header 의 오류만 확인하고 Data 의 오류는 확인하지 않는다.<br>
왜냐면 PPSC(Packet per Second) 를 높이기 위해서 Packet 당 사용 시간을 줄여야 하기 때문이다.

* 흐름제어
=> L3 에서는 안 한다. 정교한 흐름제어는 TCP 에서 해주기 때문이다.

* 혼잡제어(TCP)
network 가 감당할 수 있는 이상으로 packet 전송시 매우 많은 sender 가 정보를 보낼 때 <br>
router buffer overflow 가 발생할 수 있다. (빠른 송신자와 느린 수신자)
이 때 1. 어느정도 buffer 용량이 남았는지, 2. 찼으니까 그만 보내라 라는 신호를 보내줌.

그 외

* Routing
routing table 을 사용하는 것. (그러나 routing table 의 유지 보수는 L3 에서 하진 않는다.)

* 보안 : 모든 layer 에서 제공해줄 수 있고, 어떤 보안인지에 따라 제공하는 보안이 달라진다.
```
    5           인증서 보안제공(은행)
socket/SSL     Socket -> HTTP
     4           SSL -> HTTPs
     3           DDoS(Denial of Service) -> IP spooping 방지를 위해 
     2           WIFI 보안 제공
     1
```
IP spooping : 남의 IP 를 사용하는 것, 위조된 IP 를 알아보는 보안이 필요하다.