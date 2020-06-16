---
layout: post
title:  "Computer network 기말 정리 3"
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

목차
- Classful Addressing : 처음 사용 -> 문제점 발생했다.
- Classless Addressing : class 가 없는 주소체계
- Special addressing : 특별한 주소
- NAT (Network Address Translation, 네트워크 주소 변환)

NAT : 공유기가 local ISP 로부터 주소를 하나 받아오지만, 공유기를 중심으로<br>
사설 Network 가 형성되는데 공유기를 통해서 나갈 때 주소 변환이 필요해진다.

IPv4 에서 주소의 총 개수는 2^32 개 이다.<br>
IPv6 에서는 2^128 이다.

IPv4 는 모든 곳에서 주소를 똑같은 의미로 받아들인다. (보편적이라는 말)

<br>

### Dotted-decimal notation

Binary          10000000 00001011 00000011 00011111<br>
Dotted decimal     128  -   11   -    3   -    31

각각의 byte 를 10진법으로 읽은 것이다.

<br>

## Classful Addressing

* Network Service
1. Unicast : 1 대 1 통신
2. multicast : 1 대 many (수신자가 여러명인데, 그룹에 가입되어 있어야 한다.)
3. broadcast : 1 대 all (하나의 physical network)
4. anycast : 1 대 any (내가 가장 전송하기 쉬운 곳으로 - 가장 좋은 성능을 보장하는 곳)

* 주소를 field 별로 나누는 이유는 전송이 용이하기 위해서, r/t 을 줄이기 위해서이다.

<br>

### Netid and hostid

Netid : 제공받는 주소<br>
Hostid : 제공받는 사용자가 변경가능

```
Class A :  0  - 127
Class B : 128 - 191
Class C : 192 - 223
Class D : 224 - 239
Class E : 240 - 255
```

### Blocks in Class A

A 는 Dotted decimal 로 n1.n2.n3.n4 이렇게 있으면 (n1 은 0 ~ 127 의 자연수, n2 n3 n4 는 0 ~ 255 자연수)<br> 
n1 부분을 Netid 로 쓰는 Class 이다.

그 중에서 0.0.0.0 은 나 자신을 가리키고, 0.255.255.255 는 broadcast 주소이기 때문에 할당해서 사용할 수가 없다.<br>
127.255.255.255 가 제일 마지막으로 할당할 수 있는 Class A address 이다.

### Blocks in Class B

B 는 n1.n2.n3.n4 에서 n1.n2 를 Netid로 쓴다. n1 은 128 ~ 191, n2 는 0 ~ 255 범위이다.<br>
마찬가지로 128.0.0.0 과 128.0.255.255 는 못 쓴다.

여기서 발견할 수 있는 규칙으로는 <br>
Netid 의 최소 + hostid 의 최소 : 자기자신을 나타내는 IP<br>
Netid 의 최소 + hostid 의 최대 : Broadcast

### Blocks in Class C

n1 : 192 ~ 223, (n2, n3) : 0 ~ 255
자기자신 : 192.0.0.0
Broadcast : 192.0.0.255

### The single block in Class D

224.0.0.0 ~ 239.255.255.255

class D 의 목적은 multicast 이다.<br>
multicast 주소 : Netid, Hostid 의 구별이 없다. 그래서 실제 호스트를 나타내는 주소가 아니다.

### The single block in Class E

240.0.0.0 ~ 255.255.255.255

연구용, 미래용으로 비축해놓았다.

<br>

### Example in Class A

73.0.0.0/8 을 예로 들어보자.

73.0.0.1 ~ 73.255.255.<b>253</b> 은 일반적인 주소로 할당된다.<br>
왜 253 이냐면 73.255.255.254 는  Router 로 할당되기도 하기 때문이다. (표준은 아니다.)<br>
73.255.255.255 는 당연히 broadcast 주소이다.

<br>

## Network mask 

어디까지가 Netid 인지 알려주는 주소이다.

* 인터넷에 연결하기 위해서 필요한 정보
1. 나 자신의 IP 주소
2. DNS 서버 주소
3. Router 주소
4. <b>Netmask 주소</b>

/n 으로 주로 표기한다. (몇 비트까지 Netid 인지 표기해주는 것, n 까지를 1 로 채우기 때문에 masking 인 것 같다.)

<br>

## Subnetwork mask 

/n 을 확대해서 네트워크 과부하를 줄인다.

반대 개념인 Supernet mask 도 있다. (클래스들을 합칠 때 쓴다.)

<br>

## Classless addressing

그 중에서 
### Variable-length blocks in classless addressing 
(내 맘대로 길이)<br>
파편을 적게 하면서, 필요한 만큼 최대한 끊어줘야 한다.

IANA -> APNIC -> KISA 이런 순으로 받아온다.

<br>

### Prefix and Suffix

classless 에서 Netid 와 Hostid 역할이다.<br>
/n notation 을 그대로 쓴다.

<br>

### Example

address 개수 + 2 개의 주소 -> 2^k 로 커버해야 함

<br>

## Special Address

```
Special Address                 Netid       Hostid
Network                         Specific    All 0s
Directed Broadcast              Specific    All 1s
Limited Broadcast               All 1s      All 1s      (내가 속한 PN 에 모두 뿌림)
This host on this network (me)  All 0s      All 0s
Specific host on this network   All 0s      Specific    (같은 PN 내에서 사용가능)
Loopback                        127         Any
```

<br>

<!-- ### Example of a directed broadcast address

* 방화벽
내부망 - (F/W) - 외부망<br>
Fire Wall 은 적절하지 않은 packet 을 drop 하는 역할이다.

* Direct Broadcast 로 공격 -->


## NAT (Network Address Translation)

packet 이 바깥으로 나갈 때 주소를 바꿔주는 것.

<br>

### Example of loopback address

같은 머신 내의 여러개의 process 간의 interprocess communication 으로 사용할 수 있다.<br>
#port 가 달라서 구분할 수 있다.

같은 머신에서 다른 포트로 통신할 때 쓰는 것 같다.<br>
주로 127.0.0.1 을 관례적으로 많이 사용한다.

### NAT

NAT 가 필요한 경우
- Site 가 불법주소 또는 Private 주소를 사용 (내부망을 구성했을 때)
- Site 가 내부 네트워크 구성을 감추고 싶을 때 

내부 네트워크 구성 : Netid 가 같으면 같은 network 에 속한다. 그래서 network 의 구성형태를 아는데 도움이 된다.

- Load Balancing (Cloud machine)
- 공유기

### NAT 의 종류

- 1 to 1
- 1 to Many (PAT : Port Address Translation)