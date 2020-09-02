---
layout: post
title:  "AI framework Development 1"
date:   2020-08-31
excerpt: "이제 시작"
tag:
- AI
- framework
- blog
- silofox
comments: true
feature: https://github.com/siloFoX/siloFoX.github.io/blob/master/images/help/help.jpg?raw=true
---

ETRI에서 만든 인공지능 프레임워크인 KSB framework를 가지고 일한걸 적으려 한다.<br>
원래 이름이 BeeAI가 아니었는데 이름을 저걸로 쓰기로 했나보다.

현재는 개인팀 자격으로 공모전에 나가있다.<br>
그래서 당분간은 저걸 쓰면서 일어난 여러가지에 대해서 쓸 것 같다.<br>
많은 고난이 예상된다. 왜냐면 여러가지를 할 수 있는 만큼 복잡하기도 하기 때문이다.

내가 뭐 인공지능 엔지니어링 쪽으로 그렇게 많은 지식을 가지고 있지는 않다는 것을 감안했으면 좋겠다.<br>
보는 사람이 있다면 말이다.

<br>

# 첫번째 목표 : HTTP를 통해서 JSON 데이터 저장하기

어렵지 않은 주제이다. 평소에 Router를 통해서 데이터를 받고 저장하는 것은<br>
누구나 해봤을거라 생각한다.<br>
물론 자세히 이해하는 것은 아니고 추상적인 레벨이지만..

URL 지정 해주고 (여기서는 IP + Port지정)<br>
Content-Type을 application/json 로 설정했다.

Docker 안에서 curl을 써서 테스트하는 예제라 그런가

그래서 지금은 이메일을 기다리고 있다.

```
HttpServerReader 에 관련해서 질문드립니다.

HttpToMongoDB 샘플은 잘 돌아갑니다만 외부에서 URL을 통해
JSON 데이터를 저장해보려했으나, 방법을 잘 모르겠습니다.

메뉴얼에 보면 curl을 통해 localhost:{Port}로 전송하게 되어있는데요.
외부에서 해당 Docker로 포워딩된 Port를 찾을 수 없는 것이 문제인 것으로 보입니다.

0.0.0.0:53001 으로 설정되어있어서, PostMan 으로 {Server IP}:53001에 전송했습니다.

혹시 몰라서 Tensorflow Serving 모듈에 있는 http://csle1:8002/model 도 시도해봤지만
이것 역시 Docker container 안에 존재하는 csle1 계정으로 생성된  URI가 아닌가 합니다.
(외부 접근 방법을 모르겠다는 뜻입니다 ㅠㅠ)

혹시 외부 데이터는 Python 으로 Router를 구성해서 받아야하는 것인가요?
관련된 필요 지식이 있다면 공유해주시면 직접 습득해서 적용하도록 하겠습니다.
읽어주셔서 감사합니다. 답변 기다리겠습니다.
```
ㅠㅠ 구질구질하다.<br>
아마 내일쯤이면 답장이 오지 않을까