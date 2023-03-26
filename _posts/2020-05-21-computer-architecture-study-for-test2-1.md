---
layout: post
title:  "Computer Architecture study for test2 1"
date:   2020-05-21
excerpt: "Branch prediction buffer 와 BTB"
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

2020-05-13 Lecture 7

branch prediction 에 대해 소개하도록 하겠다.

Control hazard 를 학습할 때 branch 에 대한 결정이 늦어지면 그만큼 penalty 가 증가하는 것을 확인했다.<br>
(MEM -> ID 이동)<br>
프로그램 실행 중에 history 를 기반으로 분기를 예측하는 것이 dynamic prediction 이다.

이와 관련해서 <b>branch-prediction buffer(branch history table)</b> 와 <b>branch target buffer</b> 에 대해 설명하겠다.

# Branch prediction buffer (branch history table)

branch prediction buffer 는 branch 에 대한 예측을 저장하는 메모리이다.<br>
(예측이란 taken / not-taken 을 말하는 것, 메모리의 크기는 2^k x <b>m</b>)

물론 cache 형태로도 구현이 가능하다.<br>
Branch 명령어 주소를 index 로 찾아가서 (IF stage)<br<
저장된 <b>prediction</b> 정보(<b>m</b>)를 보고 taken / not-taken 방향을 정하는 것이다.

그런데 명령어의 주소 전체를 사용하면 공간낭비가 심하다.<br>
Branch 명령어는 일부에 불과하기도 하다.

그래서 주소의 일부 정보(ex : low-order address bits)를 index 로 사용하면 현실적인 크기의 table 로 구현이 가능하다.

당연히 두 개 이상의 branch 명령어가 동일한 index 를 갖는 경우에 같은 공간으로 연결되어서 prediction 정보가 섞일 수 있다.<br>
그러나 예측의 정확도는 저하될 수 있어도 문제는 없다.<br>
어차피 예측이니까 틀릴 수 있고 그에 대한 해결책은 구현되어 있으니까 말이다.

예측이 틀릴 경우에 flushing 하는 것을 배웠다.<br>
예측이 correct / incorrect 인지에 따라서 저장된 prediction 정보를 업데이트하게 된다.

위의 <b>m</b> 과 관련해서 1-bit prediction 과 2-bit prediction 을 예로 들어 설명한다.<br>

참고) 1-bit prediction 은 1bit(1 : taken, 0 : not-taken) 으로 관리한다.<br>
당연히 최근 branch(동일한 index 의) 가 실제로 taken / not-taken 이었는지 기록만 한다.<br>
그리고 예측이 틀리면 즉시 변경한다.

2-bit prediction 은 2bit 로 관리한다. 2 bit 이므로 4개의 상태를 가지게 된다.<br>
<b>high bit</b> 가 <b>0</b>이면 not-taken 으로 <b>1</b>이면 taken 으로 예측하게된다.
```
00 SNT (Strong Not-Taken) N2
01 WNT (Weakly Not-Taken) N1
10 WT (Weakly Taken) T1
11 ST (Strong Taken) T2
```
반드시 이 state graph 로 구현해야하는 것은 아니다.

beq 가 taken 으로 결정되면 아래쪽으로 not-taken 이면 위로 하나씩 이동한다.<br>
따라서 SNT, ST 에서는 예측이 한 번만 틀려도 약한 상태로 이동하고<br>
또 틀리면 예측의 방향을 변경하게 된다.

그 외에도 다양한 예측기법이 존재하지만 생략한다.

<br>

# Branch Target Buffer (BTB)

branch prediction buffer 가 예측 정보는 제공하지만 branch targett address(BTA) 는 제공하지 않는다.<br>
그래서 Taken branch 의 예측이 성공하더라도 BTA 계산에 1 cycle 을 소모하게 된다.<br>

이것을 제거하기 위해서 <b>branch target buffer(BTB)</b> 를 사용할 수 있다.

BTB 는 target 의 주소도 간직하는 <b>cache</b> 이다.<br>
명령어를 fetch 할 때 PC 값으로 access 하려서 prediction 정보와 target 정보를 동시에 제공받도록 구성한 것이다.

BTB 는 예측과 branch target address 가 간직되므로<br>
branch history table 같이 entry 를 공유해서는 안된다.<br>
예측은 공유해도 큰 문제가 되지 않지만 분기주소는 공유하면 안되니까??<br>

Cache 를 배운 것이 아니므로 설명이 잘 안되지만 설명한다.

<b>beq 의 주소의 일부(low-order bits)</b> 를 index 로 접근하여서, <br>
<b>Branch PC</b> 와 <b>beq 주소의 나머지 부분</b>을 <b>비교</b>한다.<br>
일치하면 hit 이므로 <b>Predicted PC</b> 를 분기주소로 사용한다.