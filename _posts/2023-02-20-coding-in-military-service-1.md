---
layout: post
title:  "Coding in military service 1"
date:   2023-02-20
excerpt: "군대에서 코딩을 해보자! 준비과정들"
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
lastmod : 2023-03-20
sitemap : 
  changefreq : never
  priority : 1.0
---

그렇다 군입대를 했다. 코딩을 할 수 없는 환경에 직면했지만, 그것은 훈련소까지 일 뿐 device 와 network 가 지원되는 한 코딩을 막을 수는 없다.
<br><br>
마음먹은 계기 : <br>
일단 당연히 구름 ide 같은 web ide 는 고려대상이지만, 나는 그냥 핸드폰 화면에서 코딩을 하고 싶고 사이버지식정보방이 없는 우리 부대 특성상 독서실 컴퓨터로 해야한다. 하지만, 조용한 분위기도 마음에 안 들고 키보드도 내가 싫어하는 타입이어서 그냥 환경을 할 수 있는만큼 구축하기로 했다.
<br><br><br>
<b>HW</b>
<br>
1. 키보드를 택배로 배송
2. mini 5 pin (male) to usb c type (male) cable
3. c type extension hub
4. 핸드폰 거치대와 그것을 부착할 수 있는 케이스
5. 모든 것들을 넣어둘 군용 가방

<br>

1 : 우선 키보드는 내 키보드 중 가장 조용하고 오래쓴 바밀로 (저소음 적축)을 부모님께 부탁드려 배송 받았다.<br><br>
2 : 그 다음에는 핸드폰에 연결하기 위한 케이블과 otg를 구비해야했는데 나무위키에 mini 5pin to usb c type 의 otg 가 지원이 안된다고 쓰여져 있는데 <br> 정확하게 말하면 c type female 이 달린 허브에(물론 이것도 c type male 연결이라 extension 용도에 가깝다.) 케이블을 연결하면 전원 공급도 잘 되고 (otg 의 기능이 데이터 전송 + 외부 기기 PD 이므로..) 잘 쓸 수 있다.<br>
c type 으로 굳이 맞춘 이유는 군대에서는 흔히 쓰는 usb(usb-a type) male 단자를 사용할 수 없기 때문이다.(압수당한다. 참고로 핸드폰 충전기에 케이블이 분리될 경우에도 마찬가지로 압수..)<br>
c type to mini 5 pin cable 을 찾는 것에도 생각보다 오래걸렸는데 그 이유는 데이터 전송이 가능한 male to male cable 이 검색하기 힘들었기 때문이다. (컴스몰의 ITB455 제품으로 선택했다.)<br>
Pipeline : phone -> hub -> cable -> keyboard
<br><br>
3 : 아마도 제일 오래걸린 부분일 것 같다. 결론부터 말하자면 SSK 사의 4-in-1 multi port hub (SC109)를 중국직구로 사게되었다. 내가 이 제품을 고른 조건은 (1) c type male 과 hub의 일체형 (2) c type female port 가 data transfer 를 지원하는지 여부(물론 otg 파트이다) (3) 가격 및 크기, 안정성 등등<br>
군대에서 사용하기 때문에 1, 2번 조건이 충족되지 않으면 살 수 없었다. 경쟁 제품이 하나 존재하였는데 elago usb c type pocket pro 라는 제품이었다. 하지만 케이스를 장착하고 쓸 수 없다는 치명적인 단점이 있어서 어쩔 수 없이 도박인 직구를 택하게 되었다. 물론 지금까지는 잘 작동해서 만족한다.<br><br>
4 : 거치대는 뭐 사실 그렇게 고심해서 살 필요는 없다고 생각했는데 기왕사는 김에 케이스를 좀 깔끔한걸로 사고싶었다. 그냥 믿고 사는 브랜드 spigen 에서 케이스 보호필름 거치대를 구매했다. 역시나 마음에 든다.<br><br>
5 : 군용 가방을 사러 군장점에 갔는데 바가지가 너무 심해서(인터넷 가격의 2배정도) 바로 인터넷으로 주문했다. 사실 군인 상대로 장사하는 곳이고 독점인거 알아서 별 기대도 안 했다.<br><br>

다음 편은 SW 부분에 대해 남길 예정이다.
요즘 with 코로나 체제로 가고나서 그것에 대한 보상심리(?) 훈련이 많아져서 쓸 시간이 많지는 않아 조금 아쉽다.