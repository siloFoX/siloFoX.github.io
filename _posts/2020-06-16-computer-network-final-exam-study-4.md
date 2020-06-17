---
layout: post
title:  "Computer network 기말 정리 4"
date:   2020-06-16
excerpt: "발등에 불 떨어진 공대생"
tag:
- computer network
- 컴퓨터 네트워크
- blog
- silofox
comments: true
feature: https://github.com/siloFoX/siloFoX.github.io/blob/master/images/computer-architecture/computer-architecture-feature.jpg?raw=true
---

## Delivery

### Direct delivery

종류
1. 같은 PN 내에서 이동 (router 를 통한 이동 X)
2. 최종 목적지 도달하는 delivery

### Indirect delivery 

Destination 이 router

<br>

## Forwarding

### Next-hop method

=> next hop & interface 사용

Destination 부분과 Route 부분이 있는데(Source 부분 말하는거 맞다.)<br>
Route 부분의 경로가 바뀔 수도 있다. 가변적인 길이이다. -> 시간 소모에 따라서 좌우된다.

그런데 Route 대신에 Next Hop 으로 하면 간단하고, 고정길이로 짧아져서 좋다.

### Network-specific method 

Network-specific routing table for host 를 기술하여서<br>
Host 별로 기록 -> 자주 변경되므로 기록하기가 번거롭다.

Network address & Netmask 방식으로 기록해 놓는다.

### Host-specific routing

=> Host 를 Routing table 에 기록하는 경우이다.

Destination | Next Hop | Interface <br>
이렇게 table 형식으로 기록해 놓는다.

### Default routing

Routing table 에서 <br>
Destination 이 없다면 Default 값에 해당하는 Next Hop 으로 이동한다.

<br>

### address aggregation

supernetting 을 말하는 것 같다. 겹치는 부분이 있으면 그냥 Mask 크기를 축소하고 합쳐버린다.

### Longest mask matching

Routing table 에서 여러개 겹치면 masking 이 최대한 많이 되어있는 것을 택한다.<br>
그러기 위해서 Routing table 에서 Mask 의 크기가 큰 순서대로 sorting 해놓는다.<br>
(여러개를 비교하는 알고리즘은 복잡하기 때문이다.)