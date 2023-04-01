---
layout: post
title:  "Learning Dart by Google 3"
date:   2023-03-31
excerpt: "Dart async"
tag:
- coding
- dart
- google
- flutter
- dart
- sync
- async
- await
- git
- github
- blog
- silofox
comments: true
lastmod : 2023-03-31
sitemap : 
  changefreq : daily
  priority : 1.0
---

Future class 는 값을 받을 변수를 지정한다. Promise 라고 생각하면 된다.<br>
예시 : 
```dart
// single variable
Future<String> name;
Future<int> number;

// Future class inner method
Future.delayed(Duration(seconds :3), () {
  print("This function excuted before 3 seconds");
});
```

밑에 읽기 전에 
callback 과 Promise,
try-catch 구문을 먼저 아는 것이 좋을 것 같다.<br>

async 와 await <br>
async 와 await 은 같이 쓰여야 한다.<br>
헷갈리는 점부터 얘기를 하자면 dart 는 애초에 비동기 이기 때문에 순차적으로 진행되지 않을거란 전제하에 생각해야한다. 순차적 프로그램에 너무 익숙해져 있어서 그렇다.<br>

async function 내부에서 await 가 동작할 수 있다. 그렇다고 꼭 return 형식이 Future 여야 하는 것은 아니다. (안에서 처리하고 끝날 수도 있는 것 아닌가.)<br>
await 를 만나게 되면 함수가 그 라인을 처리하게 전까지 멈춘다. 비동기 프로그램이라고 라인을 아래에서 위로 처리하진 않기 때문에 (오히려 위에서 아래로 처리하는데 일반적인 statement 보다 늦다면 내려가서 더 처리하는 것 뿐이다.) await 밑으로는 처리되지 않는 것이다.<br>

구글에서 찾아보면 callback 구문(then을 쓰는 꼴) 자체는 계속 쓸 수 밖에 없다. 다만 await 를 쓰는 경우에 가독성이 올라간다면 쓰는 것이 좋다. 쓰는 경우를 구분해서 쓰자는 뜻<br>

보통 동기로 처리해야하는 부분을 따로 function 으로 빼서 해야할 것 같다. main 에 async 로 하다보면 결국 동기 프로그램이 되어버리니까 말이다.<br>

stream 과 broadcast stream<br>
import "dart:async"; 해야 쓸 수 있다.
stream 은 listen 을 하나만 할 수 있고,
broadcast stream 은 여러 개가 listen 할 수 있다.