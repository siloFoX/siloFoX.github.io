---
layout: post
title:  "Learning Dart by Google"
date:   2023-03-29
excerpt: "Flutter 를 해보기 전 Dart 에 대한 얕은 공부"
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
lastmod : 2023-03-29
sitemap : 
  changefreq : daily
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


(Spread Operator, Collection If 검색해보자)
미완