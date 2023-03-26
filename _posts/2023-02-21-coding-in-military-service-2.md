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
lastmod : 2023-03-26
sitemap : 
  changefreq : weekly
  priority : 1.0
---

저번 편에 이어서 SW 관한 부분을 적으려 한다.<br>

<b>SW</b>
<br><br>
결론부터 말하자면,<br>
어플로 설치할 수 있는 IDE 를 찾아서 설치했다.<br><br>
<b>전체적인 과정</b><br>
1. Andronix 어플을 통해 리눅스 설치
2. F-droid 어플 설치
3. F-droid 를 통해서 Termux 최신 버전 설치
4. VNC viewer 설치
5. Andronix 에서 customize 한 Ubuntu(based on 22.04) 결제 (2,700₩)
6. Termux kernel 에서 Ubuntu viewer port open
7. VNC viewer 로 GUI 환경에서 사용하다가 너무 자주 틩기는 문제 발생, GUI port close
8. Ubuntu kernel 에서 여러가지 설치하다가 한글 깨짐 현상 발견
9. 커키키보드 어플 설치 및 하드웨어 키보드 연결 시 소프트웨어에서 키보드 안 보이게 설정
10. Termux 어플에서 해결점을 못 찾음
11. Termux 에 내장메모리 접근 권한을 줘서 Git 으로 코드 통제하려함
12. 만료된 Github access token 발행
13. IDE 어플 설치 (코드편집기, Spck Editor)

<br>
이러한 과정으로 현재 IDE 어플을 사용해서 편집하고있다.
<br><br>
<b>부연설명</b>
<br><br>
1 : Google 을 통해서 스마트폰에서 코딩하는 것을 찾아보니 죄다 Web IDE 를 추천해줬다. 물론 그것도 좋지만 내가 원하는 것은 스마트폰을 일반적인 PC 환경처럼(정확히는 내가 사용하는 환경 : VScode 랑 terminal 그리고 약간의 mockup test 환경) 해놓고 싶었다. 그래서 가장 편하게 생각이 난게 어차피 Android 는 Linux kernel 위에서 동작한다고 하니까 AArch64 용 Ubuntu 랑 dual booting 시키면 되겠다는 것이었다. <br><br>
그러나 (1) 안드로이드가 이미 disk 점유가 큰 상황에서 메모리 분할을 시도하는 경우에 불안정할 수 있다. (2) 연결할 수 있는 내장메모리가 존재하지 않는다. (이 때는 아직 Hub 를 구매하기 전이고, 설사 구매했더라도 군대에 내장메모리는 반출이 불가능하다.) (3) 과거에 스마트폰에서 그 일을 시도해본 적이 없어서 백업이 필요하다고 생각되었는데, 군대에서 진행할 수 없는 일이다. <br><br>
그래서 WSL 같이 안드로이드 내에서 리눅스를 설치하는 어플이 존재하지 않을까 했는데 역시 있었다. 그 어플이 Andronix 였다. Playstore 에서 설치했다. 그리고 Termux 도 같이 세트로 설치해야한다는 것을 알게되었다.
(Andronix 는 그냥 installer, Termux 는 Terminal) <br><br>
2: Termux 를 설치해야하는데 Playstore 에 있는 어플을 설치하고 버전을 확인해보니 버전이 너무 낮았다. 찾아보니 F-droid 라는 어플을 설치하였는데 Playstore 에 있는 어플이 아니었고 개발자 옵션을 켜서 미승인 어플을 설치할 수 있게 해놓은 뒤에 설치할 수 있었다. <br><br>
3 : F-droid 에서 Termux 를 설치
4 : Playstore 에서 VNC port 를 통해 Linux server 를 제어할 수 있는 VNC Viewer 어플 설치
5 : Andronix 에서 Ubuntu 를 설치하려 하는데 무료버전을 깔려고 계속 시도하는데 중간에 Andronix 에서 {script} 를 clipboard 에 복사해주는 구간이 있다. 별건 아니고 Termux 에서 접근 할 수 있는 home/ 에다가 Andronix 가 Ubuntu 설치파일을 가져다 놓았으니 ./{script} 하라는 것이었다. 그러나 몇 번 시도를 해봐도 설치파일이 없는 것이었다. <br><br>
그래서 유료버전을 설치하라는 미끼상품인건가 싶어서 한번 속는셈치고 거금 2,700₩을 들여서 (군인에게는 거금이다.) 설치를 진행했는데 잘 설치가 되었다. Andronix 에서 customize 한 Ubuntu 이고 base version 은 22.04 LTS for AArch64 이다.<br><br>
6 : Termux 에서 Ubuntu 설치를 하고 기본적인 설정 이것저것하고 나니 안에 이미 vnc port 를 여는 package 가 설치되어있었다. 그래서 port 를 열고 닫는 명령어를 대충 익히고 VNC viewer 로 이동해 익숙한 Ubuntu 환경을 즐기고 해피엔딩 일 줄 알았으나.. <br><br>
7 : 일단 VNC Viewer 어플이 다운로드 숫자가 500만이 넘었길래 Team viewer 만큼은 아니어도 어느정도 편의성은 있을 줄 알았다. 그러나 하드웨어 마우스를 이용하지 않으면 사용이 너무 불편한 GUI 와 자주 틩기는 불안정성 때문에 10분에 한번씩 Ubuntu 를 켜야되는 수고를 겪었다. 물론 불안정성 이슈는 Andronix 에서 제공하는 Ubuntu 때문일 가능성이 크지만 아무튼 사용하기 여간 어려운 것이 아니었다. 그리고 결정적으로 한글깨짐 현상을 해결하려고 할 때 설치하는 도중에 계속 Connection 이 꺼져서 설치를 진행할 수 없다고 판단했다. <br><br>
8 : 이렇게 된 이상 VNC port 를 이용하는 것은 포기하고 Termux 내에서 제어하는 것으로 생각이 바뀌었다. 그리고 마찬가지로 Nanum font 와 font manager 를 설치하고 적용해보았는데 키보드로 입력이 되지 않았다. <br><br>
9 : 한글 출력이 갤럭시 기본키보드 설정 때문에 안되는가 싶기도 하고, HW 키보드 사용하는데 불편한 점이 여러개 있어서 커키키보드라는 어플을 깔아서 사용해보았다. 그리고 Termux 에 입력해보았는데, blank 를 포함한 한글이 입력되었다. 역시나 buffer 문제도 있었다. 동시에 설정에서 HW 키보드를 연결했을 때 UI 상에서 키보드가 보이지 않게 설정도 할 수 있었다. (이건 그냥 안드로이드 키보드 고유기능) <br><br>
10 : 불편하지만 그래도 나름대로 사용할 수 있기 때문에 Git 으로 repo 를 가져와서 VI 로 파일을 열었는데 한글이 다 깨져있었다. 그래서 다시 여러가지 Termux 설정도 찾아보고 인코딩도 찾아보다가 결국 중단했다. <br><br>
11 : 결국 IDE 어플을 사용해서 코드를 편집하기로 결정했다. 그러려면 내장메모리에 파일을 넣고 Git 으로 Github 에 접속하는 것이 좋을 것 같았다. 그 전에 Git 을 사용하기 위해서 Termux 에 내장메모리 접근 권한을 주었다. <br><br>
12 : 내가 Github repo 관리자이므로 pull request 를 굳이 할 필요 없이 바로 push 할 수 있게 Github token 을 local 에 저장하려했는데 예전 token 이 만료된 모양인지 접속이 되지 않아서 다시 발행했다. <br><br>
13 : 마지막으로 code 수정을 할 수 있는 IDE 를 설치하려하는데 두가지 중 하나로 좁혀졌다. <br> (1) '코드편집기' 라는 어플이다. 이 어플은 내장메모리 접근해서 파일을 불러올 수 있다. 그리고 가로로 사용할 수 있다. 문제는 가끔 광고가 뜨는데 이 광고를 제거하려면 4,900￦ 이 든다. <br> (2) 'Spck Editor' 라는 어플이다. 이 어플은 내장메모리는 접근하지 않는 것 같다. 따로 분리된 어플 내에 메모리로 관리하는 것 같다. 대신 Github repo 에서 바로 clone 해올 수 있고 VScode 와 비슷한 UI 와 기능을 제공한다. 상당히 비슷하다. 몇가지 소개하자면 파일구조 볼 수 있는 것, 검색기능, branch 관리, push 저장소 auth 제공, 비슷한 theme 제공, 자동저장 등이 있는데 Node.js 에 특화되어있는 점도 있다. Node 는 따로 테스트 해보진 않았지만 tutorial 이 JS를 실행시켜보는 것이었다. (다만 Node.js Terminal 은 Basic-무료 요금제에서 1시간만 가능하다고 써있다.) 단점은 가로모드를 지원하지 않고 (display ratio 호환성 문제인 듯) shortcuts 를 임의로 바꿀 수 없다는 것이다. <br><br>

그렇게 해서 최종적으로 Spck Editor 에서 이 post 도 작성하고 있다. 생각보다 제약사항이 많은 것 같았다. 정보처리기사 공부도 하고 만들고 싶은 것이 생기면 이것저것 만들어볼 예정이다. 지금 생각으로는 청소표를 알아서 짜주는 프로그램 정도 생각 중이다.<br>

2023-03-17 현재 키보드와 OTG 승인을 마쳤다.
다음은 이 Blog 를 검색 시스템에 노출시켜봐야겠다.<br>

2023-03-26 핸드폰 환경이 키보드에 friendly 하지 않다는 것이 불편사항으로 되어서 이번에는 Pointing device 로 바꾸기로 했다. Mouse 를 샀다는 뜻이다.