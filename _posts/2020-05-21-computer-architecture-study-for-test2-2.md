---
layout: post
title:  "Computer Architecture study for test2 2"
date:   2020-05-21
excerpt: "메모리계층구조와 Cache 설계"
tag:
- computer architecture
- 컴퓨터 구조
- blog
- silofox
comments: true
lastmod : 2020-05-21
sitemap : 
  changefreq : never
  priority : 1.0
feature: https://github.com/siloFoX/siloFoX.github.io/blob/master/images/computer-architecture/computer-architecture-feature.jpg?raw=true
---

2020-05-15 Lecture 8

프로세서는 속도를 중요시해 발전되었다면, 메모리는 용량과 가격이 중시된 경향이 있다.<br>
따라서 프로세서와 메모리의 속도차를 어떻게 극복하는냐가 메모리시스템 설계의 중요한 문제이다.

우리는 속도가 빠르고 용량도 크면서 가격이 저렴한 메모리를 원한다.<br>
속도도 빠르고 용량이 큰 메모리가 있는 것 같은 효과를 갖는 <b>메모리계층구조</b>를 배워보자.

4단계로 진행된다.
1. memory hierarchy(Cache - Main memory - Disk) 에 대한 개념
2. cache-main memory 계층과 main memory-disk 계층으로 나눠 구체적인 설명
3. cache-main memory 계층(<b>cache design</b>)에서 프로세서 메모리 access request 가 <b>물리주소</b>로 주어진다고 가정하고 설계의 중요한 이슈 소개
4. main-memory disk 계층으로 이동하여 virtual memory 를 개념적으로 정리

물리주소는 main memory access 에 사용된다.<br>
virtual memory 의 경우에 프로세서는 <b>virtual address</b> 로 memory request 를 생성한다.

3번째 단계에서 physical address 로 가정한 cache 설계에 변화가 있을 수 있다.<br>
그래서 마지막으로 cache 설계로 돌아와서 그 영향에 대한 것을 배운다.

4단계 과정이 끝나면 memory request 가 처리되는 과정을 cache, main memory, disk 모두 연결된 상태의 이해가 가능하게 된다.

<br>

# 메모리계층구조

메모리계층구조란 피라미드 형태의 메모리 결합을 의미한다.
```
  /cache
 /main memory
/disk storage
```
상대적으로 위쪽은 용량이 작고, 속도는 빠르나 단위 bit 단 가격이 높은 메모리이다.<br>
속도는 cache, memory, disk 순<br>
용량은 역순이다.

이와 같은 계층적인 구조가 프로세서의 request 들에 적절하게 대응할 수 있을지 의문이 들기도 한다.<br>
주소가 난수형태로 발생된다면 최근에 접근한 data 를 상위의 빠른 메모리에 저장하더라도 불가능하기 때문이다.

그러나 프로그램은 <b>시간적 지역성(temporal locality)</b> 와 <b>공간적 지역성(spatial locality)</b> 가 있어서 가능하다.<br>
임의의 난수 발생이 아니라는 뜻이다.

이를 <b>지역성의 원칙(principle of locality)</b>라고 한다.

<br>

<b>Temporal locality(시간적 지역성)</b> 는 한번 access 한 address 는 가까운 미래에 다시 access 할 가능성이 높다는 것이다.<br>
그리고 <b>Spatial locality(공간적 지역성)</b> 는 한번 access 한 address 주변은 가까운 미래에 access 할 가능성이 높다는 뜻이다.<br>

이는 프로그램이 가지는 지역성이므로 현재 집중적으로 접근하는 일부만 빠른 cache 에 보관하고 있어도<br>
대부분 메모리를 cache 에서 공급이 가능하게 된다는 것이다.

<br>

# Cache 설계

Cache 의 존재는 programmer 에게는 감춰진 것으로 볼 수 있다.<br>
프로세서의 메모리 request는 cache controller 가 중간에 개입하여 cache 에 있는 경우 <b>(cache hit)</b><br>
직접 보내고 없는 경우에는 <b>(cache miss)</b> 메모리로 request 를 보내게 된다.

프로세서가 명령어나 data 를 요청했을 때 그 명령어 혹은 data 만 가져와 cache 에 저장할 수 있는 경우<br>
시간적인 locality 는 활용하지만 공간적인 locality 는 살리지 못 한 것이다.<br>
따라서 cache 에 없는 경우에 main memory 에서 해당 data 를 가져오는데,<br>
<b>그 data 를 포함한 block (즉, line)</b> 을 가져와 cache 에 저장하게 된다.

<br>

# Cache 설계의 중요 이슈

1. cache miss 가 발생하면 메모리에서 data 를 포함한 block 을 가져오게 되는데 cache 내에 어디에 저장할까<b>(block placement)</b>
2. 프로세서가 memory request 에 대해 cache controller 가 자신이 가지고 있는지 확인하는 과정<b>(block identification)</b>
3. 새로운 block 이 cache 로 들어와야 할 때 cache 의 공간이 부족한 경우 기존의 block 을 추방하는 방식<b>(block replacement)</b>
4. 메모리 write request 에 대한 처리 방식<b>(write strategy)</b>

4번에서는 write 할 경우 cache 에만 아니면 cache 와 메모리 모두 write 하는지 문제도 포함된다.


# 예시 

Cache 의 크기는 4 bytes, main memory 는 16 bytes, block size 는 1 byte 로 가정하자.<br>
공간적 지역성은 활용하지 못 하겠지만 시작으로서는 괜찮다.

프로세서가 request 한 주소의 data 가 cache 에 없으면<br>
memory 에서 그 byte 를 포함하는 block 을 가져와 cache 에 저장하게 될 것이다.<br>
(물론 여기서는 그냥 byte 를 가져오겠지 block 단위가 byte 이니까)

Block placement(mapping) 은 3가지 <b>(direct-mapped, (fully) associative, set-associative)</b>로 분류하는데,
여기서는 첫번째 direct-mapped 를 사용한다.(추후에 설명함)<br>
```
    Index = (block 주소) mod (cache 의 전체 block 수)
```

Cache 는 4 개의 block을 가질 수 있으므로<br>
주소를 % 4 인 값으로 주소를 찾는다. 

메모리 <b>블록 주소</b>가 0110 인 경우에 <br>
메모리 블록 <b>0110</b> 이 cache 로 들어오면 direct-mapped 의 경우에 반드시 <b>10</b> 에 위치하게 된다.<br>
(01 을 mod 한 값이므로)

그리고 cache 에서는 추가적으로 tag 라는 정보를 부착하여서 관리한다. <b>01</b> 이라는 값이 저장되는 것이다.<br>
그러면 비로소 tag + index 로 01 + 10 이 확인되는 것이다.

그런데 cache block 에 유효한 데이터가 저장된 것인지도 표기해야 한다.<br>
처음 시작 할 때는 유효한 데이터가 없기 때문이다.

그래서 tag 옆에 <b>valid bit</b> 가 추가되었다. <b>Valid bit = 1</b> 인 경우에 유효한 데이터로 간주한다.

다음에는 Mapping 을 소개하게 된다.