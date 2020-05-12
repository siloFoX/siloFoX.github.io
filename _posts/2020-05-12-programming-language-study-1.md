---
layout: post
title:  "Programming language 1"
date:   2020-05-12
excerpt: "Structured Control 의 탄생 배경"
tag:
- PL
- programming language
- 프로그래밍 언어
- 포트란
- Fortran
- Structured Control
- BNF
- Backus Normal Form
- Backus-Naur Form
- 단말 기호
- termninal
- Context-free Grammar
- CFG
- 문맥 자유 문법
- blog
- silofox
comments: true
feature: https://github.com/siloFoX/siloFoX.github.io/blob/master/images/programming-language/programming-language-feature.jpg?raw=true
---

(2020) 5월 12일 중간고사가 끝나고 다시 수업에 들어갔다.

오늘 수업 자료

<br>

# Control

## 9.1 Structured Control

### Spagehetti Code

    10  IF (x, GT, 0.000001) GO TO 20
        X = -X
    11  Y = X*X -SIN(Y)/(x-1)
        IF (X, LT, 0.00001) GO TO 50
    20  IF (X*Y, LT, 0.00001) GO TO 30
        X = X - Y - Y
    30  X = X + Y
        ...
    50  CONTINUE
        X = A
        Y = B - A + C * C
        GO TO 11

<br>

>
#### 수업 내용
이 코드는 Fortran 이라는 언어로 구성되어 있다.<br>
BNF(Backus Normal Form) 을 만든 사람이 이 언어의 컴파일러를 만들었다.<br>
column 제한이 있다 : 1 줄이 80 칸이다.<br>
그 이유는 프로그램을 입력할 때 card (A4 용지의 1/4 크기)를 썼기 때문이다.<br>
(아마 천공카드를 의미하는 듯 하다.)<br><br>
Condition(조건문)이 거의 Assembly 수준이다.<br>
branch(분기)는 강력한 Control primitive 인 GO TO 를 썼다.<br>
countinue 는 loop 문의 끝에 놔두기도 했다고 한다.<br><br>
<b>GO TO 문이 문제다 branch 하는 과정에서 국수같이 이리갔다 저리갔다 한다.<br>
<b>꼬이기 때문에 Spagehetti Code 라고 한 것이다.<br>
<b>한 눈에 들어오지 않기 떄문에 debugging 이 힘들고 오류 자체도 많이 났다.<br>
<b>한마디로 Control 이 엉킨 것이다.<br><br>
>

<br>

### 추가조사

#### Fortran

1950년대 말에 IBM 에서 John Backus 외 6명의 전문가가 만들었다.<br>
최초의 고급 프로그래밍 언어 중 하나이다.<br>

과학 계산용으로 주로 사용된다. 그 이유는 수식 계산에 특화되어 있기 때문이란다.<br>
수퍼컴퓨터 연산용으로 많이 쓰인다고 한다.<br>

또 다른 특징으로는 산술 기호를 그대로 사용할 수 있다는 것이다.<br>
(어셈블리를 하다보면 이것이 얼마나 좋은 것인지 체감할 수 있다. ㅠㅠ)<br>

기초적인 수학 함수들을 불러내어 쓸 수 있다. (삼각-지수-대수 등등)<br>
배열의 배치 순서가 다르다. Column-Major Order 라고 한다.<br>
배열의 index 를 마음대로 조정할 수 있다.<br>

초기 버전은 대문자만 쓸 수 있었다. <br>
왜냐면 그 때 당시에는 키보드가 없어서, 천공카드로 프로그램을 입력했기 떄문이다..<br>
(천공카드에는 소문자가 없다고 하더라..)<br>

띄어쓰기는 무시하지만, 줄바꿈은 의미를 가진다.

#### BNF

Backus Normal Form 또는 Backus-Naur Form 이라고 한다.<br>
문맥에 무관한 문법(문맥 자유 문법)을 만들기 위한 표기법이다.<br>

문맥 자유 문법(Context-Free Grammar, CCG)이란 프로그래밍 언어의 정의에 사용된 문법 중 하나인데,<br>
비말단 기호(V)에서 비단말과 단말 기호로 이뤄진 문자열(W)로 변환하는 생성 규칙을 따르는 문법이다.<br>
<b>단말 (Terminal)</b> 이 뭔지는 밑에서 다루도록 하겠다.<br>
문맥 자유 문법의 기호로서 정의는 V -> w 이다.<br>

쉽게 말하자면 BNF 는 <br>
프로그래밍 언어에서의 문법을 수학적인 수식으로 나타낼 때 사용하는 도구이다.<br>

여러가지 메타기호를 이용해서 나타낸다.<br>
(참고 : 메타- 란 어떤 것 스스로에 대해 설명하는 것을 의미한다. <br>
그러므로 메타기호란 기호를 설명하는 기호인 것이다.)<br>

- ::= 는 <b>정의</b>를 의미한다.
- | 는 <b>or</b> 의 의미이다.
- <> 는 <b>비단말 기호</b>이다.

일단 예시를 보자 <br>
```js
1 <digit> ::= 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9
2 <number> ::= <digit> | <number><digit>
```
::= 는 왼쪽에 있는 것을 오른쪽으로 바꿀 수 있다는 뜻이다.<br>
1, 2 번 줄은 각각 정수를 생성하는 생성규칙이고, 이것들이 모여서 BNF 를 이루게 된다.<br>

정수를 생성한다는 말이 무슨 뜻일까?<br>
2020이라는 정수를 생성한다고 생각해보자.<br>

```js
<number> -> <number><digit>               // 2번의 2번째 생성규칙
         -> <number><digit><digit>        // 2번 2번째 생성규칙
         -> <number><digit><digit><digit> // 2번의 2번째 생성규칙
         -> <digit><digit><digit><digit>  // 2번의 1번째 생성규칙
         -> 2020                          // 1번 생성규칙
```
이렇게 생성하게 된다.<br>
여기서 <> 안에 있는 number 나 digit 같은<br>
최종결과(단말 문자열)을 만들어 내기 위한 기호이고,<br>
이것을 <b>비단말 기호</b>라고 한다.<br>

또한<br>
0 ~ 9 까지는 비단말 기호 digit 으로부터 나온<br>
<b>단말 기호</b> 이다.<br>

위에서 문맥 자유 문법의 정의가<br>
비단말 -> 비단말 + 단말 의 생성규칙을 지키는 문법이라고 했으니<br>
위에 생성규칙은 그것을 만족한다.<br>

정리하자면 저 문맥 자유 문법을 만들기 위한 <b>표기법</b> 자체를 BNF 라고 한다.<br>

인터넷에서 찾은 또 다른 예시
```
<if문> ::= if<논리식>then<문장> | if<논리식>then<문장>else<문장>
<조건문> ::= <if문> | <while문> | ...
```
컴공이면 이 예시가 더 이해가 갈 수 있을 것 같다.

<br>

### 9.1.2 Structured Control 

Fundamental question : What should be done if control transfer to the middle of a block? <br>
Pascal in 1970s made it common to use simple control structure.<br>

- if-then
- if-then-else
- while-do / do-while / repeat-until
- for
- switch (case)

<br>

>
#### 해석
근본적인 질문 : 제어가 블록 중간으로 이동시키면 어떻게 해야하나?<br>
1970년대에 Pascal 은 간단한 제어 구조를 사용하여서 이것을 일반화 했다.<br>
#### 수업 내용
goto 는 Dijkstra 가 말했듯 harmful 하다.<br>
goto 는 block 안으로도 jump 할 수 있는데, 그것이 더욱 복잡한 코드를 만들 수 있게 된다.<br><br>
그래서 Pascal 을 기점으로 Structured control 이 나왔다.<br>
goto 는 아무 곳으로나 움직일 수 있었다.<br>
그러나 goto 없어도 Turing complete 할 수 있음이 밝혀지게 되었다.<br>
(partial recursive function 이 가능하다는 것)<br><br>
Structured control 의 분기는
- if-then 
- if-then-else
- while-do / do-while / repeat-until
- for
- switch (case)
>
>
이렇게 있다. <br>
block 에 들어오는 것은 무조건 블록의 시작에서만 가능하게 제한을 건 것이다.<br>
(물론 C 에서는 goto 가 있기 떄문에 예외다.)<br>
(그리고 나가는 것은 그냥 쉽게 해놨다. ex : break)<br><br><br>
control 에서 가장 중요한 것은 sequential execution 이다.<br>
high level language 에서는 statement by statement.<br>
>

<br>

### 추가조사

#### Structured control

구글링 해본 결과 Structured programming 이라고 더 많이 부르는 것 같다.<br>
절차적 프로그래밍의 하위 개념이다.<br>
goto 문을 없애거나 goto문에 대한 의존성을 줄여주는 것으로 가장 유명하다.<br>

#### Partial recursive function

이 동영상 강의를 보면 될 것 같다.
<iframe width="560" height="315" src="https://www.youtube.com/embed/yaDQrOUK-KY" frameborder="0"> </iframe>