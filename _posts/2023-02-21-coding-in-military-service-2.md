---
layout: post
title:  "Coding in military service 2"
date:   2023-02-21
excerpt: "Android 에서 코딩하기 위한 준비"
tag:
- military
- coding
- android
- termux
- vnc
- ubuntu
- 커키키보드
- git
- github
- ide
- 코드편집기
- spck editor
- mini 5 pin
- micro 5 pin
- usb-c
- usb c
- c type
- spigen
- blog
- silofox
comments: true
---

저번 편에 이어서 SW 관한 부분을 적으려 한다.<br>

<b>SW</b>
<br>
결론부터 말하자면,<br>
어플로 설치할 수 있는 IDE 를 찾아서 설치했다.<br>
전체적인 과정<br>
1. Andronix 어플을 통해 리눅스 설치
2. F-droid 어플 설치
3. F-droid 를 통해서 Termux 최신 버전 설치
4. VNC viewer 설치
5. Andronix 에서 customize 한 Ubuntu(based on 22.04) 결제 (2,700₩)
6. Termux kernel 에서 Ubuntu viewer port open
7. VNC viewer 로 GUI 환경에서 사용하다가 너무 자주 틩기는 문제 발생
8. GUI port close
9. Ubuntu kernel 에서 여러가지 설치하다가 한글 깨짐 현상 발견
10. 커키키보드 어플 설치 및 하드웨어 키보드 연결 시 소프트웨어에서 키보드 안 보이게 설정
10. Termux 어플에서 해결점을 못 찾음
11. Termux 에 내장메모리 접근 권한을 줘서 Git 으로 코드 통제하려함
12. 만료된 Github access token 발행
13. IDE 어플 설치 (코드편집기, Spck Editor)

<br>
이러한 과정으로 현재 IDE 어플을 사용해서 편집하고있다.
<br>
<b>부연설명</b>
<br>
1 : Google 을 통해서 스마트폰에서 코딩하는 것을 찾아보니 죄다 Web IDE 를 추천해줬다. 물론 그것도 좋지만 내가 원하는 것은 스마트폰을 일반적인 PC 환경처럼(정확히는 내가 사용하는 환경 : VScode 랑 terminal 그리고 약간의 mockup test 환경) 해놓고 싶었다. 그래서 가장 편하게 생각이 난게 어차피 Android 는 Linux kernel 위에서 동작한다고 하니까 AArch64 용 Ubuntu 랑 dual booting 시키면 되겠다는 것이었다. <br>

<br>
(미완)