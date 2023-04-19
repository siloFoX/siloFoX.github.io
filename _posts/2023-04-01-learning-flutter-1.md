---
layout: post
title:  "Learning Flutter by Google 1"
date:   2023-04-01
excerpt: "Flutter 기본 정보"
tag:
- coding
- google
- flutter
- dart
- git
- github
- blog
- silofox
comments: true
lastmod : 2023-04-01
sitemap : 
  changefreq : never
  priority : 1.0
---

React Native.. 과거에 나도 써볼까 고민했던 cross platform framework 이다. 하지만 Google 이 밀어주고 있고 트렌드이기 때문에 Flutter 를 선택했다.<br>

Flutter 는 Skia 라는 2D rendering engine 과 직접 통신을 하기 때문에 UI 가 일관되어서 platform 별로 수행해야하는 debugging 부담이 적다.<br>

Flutter 가 실행되는 계층구조이다.
```
Framework : 우리가 여기서 개발
   ||
Engine : C++ 로 작성된 core API, file system, Skia, network ..
   ||
Embedder : low level,  OS 자체 기능을 modulization ..
```
Skia 가 지원되는 모든 것들에 적용이 가능하다. Windows, Mac, Linux, Web 에도 배포가 가능하다는 뜻이다.<br>

우선 desktop 환경에 Android studio 와 Cmake build tool 을 위해 Visual Studio 를 설치 (2019 tool 때문에.. 이젠 컴퓨터 용량보다 내가 수고하는 시간이 더 아깝다) Flutter 환경변수 설정도 하였다.<br>

이제 본격적으로 Flutter 로 들어가보도록 하겠다.