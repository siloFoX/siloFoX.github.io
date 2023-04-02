---
layout: post
title:  "Learning Dart by Google 1"
date:   2023-03-29
excerpt: "Flutter 를 해보기 전 Dart 에 대한 얕은 공부 1 기본 문법"
tag:
- coding
- dart
- google
- flutter
- git
- github
- blog
- silofox
comments: true
lastmod : 2023-03-31
sitemap : 
  changefreq : weekly
  priority : 1.0
---

친구가 어플개발하자는 연락을 받았다. 디자인 시안까지 보낸 것으로 봐서 진심인 것 같았다. 마침 심심했기도 하고 목표로 삼기 좋은 것 같기도 해서 개발해보기로 했다.<br>

어플 개발은 처음이기도 하고 추천을 많이 받기도 해서 Flutter by Google 을 framework 로 골랐다. 그래서 Dart 언어를 먼저 쭉 훑어보기로 했다. 익숙한 OOP 의 향기가 났다.<br>

책은 장병지원을 80% 받아서 알라딘으로 주문했다. 책 이름은 <코드팩토리의 플러터 프로그래밍> 이고, 버전에 좀 덜 영향 받기 위해서 가장 최근에 나온 책을 그냥 선택했다.<br>

인터넷에 자료가 방대한데 굳이 책을 산 이유는 군대라는 특수한 환경 때문에 듀얼모니터를 쓰기 힘들기도 하고, 책으로 정제된 지식을 받아들이는건 조금 느낌이 다르기 때문에 튜토리얼은 책으로 하는게 좋다고 생각하기 때문이다. 가소성 문제도 있고 말이다.<br>

Dart 에 대한 소개에서 생각난 것들
- 비동기 언어이구나 (JS가 생각났다.) Isolate 로 동시성을 제공.
- Spread Operator, Collection If 가 있어서 효율적이라는데 그게 뭘까
- AOT compile 가능 -> C/C++ 처럼 경량화/배포 가능하겠군
- JS 로 완전한 compile 지원 (신기하다. 완전 대체 가능하게 만들었다고 하더니 역시 구글)

spread operator 는 Iteratable 객체 안에서 구성하는 객체 각각을 펼치는 연산자 이다.
string object 로 예를 들자면 "spread" 를 펼치면 
's', 'p', 'r', 'e', 'a', 'd' 가 되는 것이다.<br>

collection if 는 Iterable 객체 안에서 if 를 사용할 수 있어서 T/F 여부로 객체를 넣었다 뺐다 할 수 있는 것이다. (for 도 있다.)<br>

상수 지정 방식에는 2가지가 있는데 final 과 const 가 있다.
final 은 런타임에서 정의된다. const 는 빌드타임 때 정의되기 때문에 컴파일 때 값이 나와야 한다.
NULL 값에 대해서 어떻게 반응하는지 final 이나 const 뒤에 당연히 datatype 을 붙일 수 있지 않을까 공부하면서 알아보려한다.<br> <= 당연히 넣을 수 있었다. (2023-03-31 수정)<br>

Collection type 에는 List, Map, Set 이 있는데 Set 은 말 그대로 중복 없는 값들의 집합을 의미 한다.
Collection type 은 서로의 타입으로 형변환이 자유롭다고 한다.<br>

Iterable 은 abstract class 로 Collection type 들의 parent class 이다. 순서가 있는 값을 반환할 때 사용한다. 출력했을 때 소괄호의 형태로 출력된다.<br>

익명함수에서 single statement 로 그냥 return 값이 정해지면 => 으로 처리한다.<br>
(name) => name == "우영" || name == "우엉"
이런 식이다.<br>

List methods<br>
map, reduce 지원 fold 라는 reduce 의 변형된 꼴도 있다.<br>
arr.fold<int>(0, (value, element) => value + element.length)
이런 식이다.<br>

datatype 뒤에 ? 를 추가해줘야 null 값이 들어갈 수 있다.<br>
variable 에 ??= operator 를 붙이면 기존 값이 null 값일 때만 저장된다.<br>

is 는 type 비교 operator 이다.

positional parameter 는 대괄호 이고, named parameter 를 쓰려면 중괄호와 required 키워드를 써야한다.
required 는 null 이 불가능한 type 이면 기본값을 지정해주거나 필수로 입력해야한다는 뜻이라고 한다.<br>

positional 이랑 그냥 일반 parameter 랑 다른 점이 뭔지 궁금하다. 찾아보니 positional, named parameter 전부 optional parameter 였다. 그래서 따로 위치를 고정해야할 때나 이름을 명시해야할 때 required 를 활용하는 것 같다.<br>

anonymous function 과 rambda function 을 따로 구분하지 않는다. 쓸 수는 있으나 형태만 다르고 처리는 똑같다는 뜻 같다.<br>

typedef 로 함수의 원형을 선언하고(signature 라고 하는 듯) 값으로도 사용할 수 있음. dart 에서 function 은 first-class citizen 임.<br>

참고 : first-class citizen 이란 일반적으로 지원하는 operator 를 모두 지원하는 객체를 뜻한다.<br>

다음 포스팅에는 OOP 얘기를 하겠다.