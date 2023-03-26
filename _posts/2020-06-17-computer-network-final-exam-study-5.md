---
layout: post
title:  "Computer network 기말 정리 5"
date:   2020-06-17
excerpt: "발등에 불 떨어진 공대생"
tag:
- computer network
- 컴퓨터 네트워크
- blog
- silofox
comments: true
lastmod : 2020-06-17
sitemap : 
  changefreq : never
  priority : 1.0
feature: https://github.com/siloFoX/siloFoX.github.io/blob/master/images/computer-architecture/computer-architecture-feature.jpg?raw=true
---

미완

```
7장 ip diagram에 이름이랑 역할은 알아둬야할듯?
어떤 부분을 통해서 같은 datagram에서 나왔는지 알수있는가?
service type도 물었었구
fragmentation하는거도 나왓어 그래서 m이 뭐고 패킷길이 어떻게 되는지?
그리고 strict-sourced-route option에서 거쳐서 이동할때 패킷에 출발지 목적지 어떻게 되는지 다 쓰라고 나옴
```

아는 분의 도움을 받아 잘 공부해보자

### Position of IP in TCP/IP protocol suite

Network layer 에서 <br>
IGMP : multicasting 에서 사용한다. -> 수신자들의 가입 상태를 확인할 때 사용한다.<br>
ICMP : 오류제어에 사용한다.

<br>

## IP datagram

header (20 - 60 bytes) | Data (65,535 bytes)

header 에서만 오류탐지 기능을 적용. router 에 부하를 줄이기 위함이다.

header 의 길이를 알아야지만 data 의 길이를 알 수 있다. 

Data length = total length - header length * 4

<!-- ```
0    3  4       7  8             15 16         31
VER     HLEN       Service type     Total length
4 bits  4 bits     8 bits           16 bits

        Identifiacation
        (같은 데이터그램에서 
        나왔다는 것을 알려주는 필드)
        16 bits -->