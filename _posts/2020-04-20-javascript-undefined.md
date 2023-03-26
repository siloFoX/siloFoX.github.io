---
layout: post
title:  "JavaScript undefined"
date:   2020-04-20
excerpt: "JavaScript undefined 에 관한 간단한 지식"
tag:
- Javascript
- JS
- undefined
- first post
- silofox
- "null"
comments: true
lastmod : 2020-04-20
sitemap : 
  changefreq : never
  priority : 1.0
feature : true
---

## JavaScript undefined 가 무엇일까?

<br>
우선 코드부터

```javascript
var x;
x = "abcde";
console.log(x);
```

한줄씩 간단하게 짚고 넘어가겠습니다.
<br><br>

1. var x;

   x라는 변수를 선언합니다.

   변수란 어떤 값을 담기위한 상자같은 존재입니다.
   상자를 만들고 x라는 이름을 붙여준 것과 같습니다.
<br>

2. x = "abcde";

   x에 "abcde"를 넣습니다.

   대부분 프로그래밍 언어에서 '='은 대입의 의미를 갖습니다.
   x라는 상자 안에 "abcde" 라는 것을 넣어줍니다.
<br>

3. console.log(x);

   x를 출력합니다.
<br><br>

따라서 결과는

```javascript
abcde
```

라고 나오게 됩니다.
<br><br>

요약하면 x 라는 상자를 만들고<br>
그 안에 "abcde" 라는 데이터를 넣은 다음<br>
x 상자의 내용을 출력한 것이지요.
<br><br>

```javascript
var x;
console.log(x);
```

그런데 이렇게 x 상자에 내용을 넣지 않고 출력하면 어떻게 될까요?
<br><br>

예상하셨겠지만, 

```javascript
undefined
```

undefined 가 나옵니다. 
<br><br>

---
<br>

사실 프로그래밍을 해보신 분들이라면 (그리고 JS가 처음이시면..)  뭔가 이상하실 수도 있을 것 같습니다.<br>
보통 초기화를 하지 않은 변수(대입을 하지 않은 변수)를 출력하려 하면 오류가 뜨고 실행이 안되는 것이 정상이니까요.<br>
그러나 JS는 웹프로그래밍을 목적으로 만들어진 언어입니다. <br>
저런 자그마한(?) 오류 때문에 웹 자체가 실행이 멈춰버리면 난감한 상황이 발생하겠죠.<br>
그래서 JS에서는 저런 초기화하지 않은 변수에 임의로 undefined를 넣어버린 것입니다.<br>
재밌는 점은 type을 체크해도 undefined 로 나옵니다. type 자체도 정해지지 않았다는 것이죠.<br>
이 부분에서 null 과 다른 점이 발생합니다. 
<br><br><br>

null 과의 자세한 차이점은 여기 블로그를 참고하시면 좋을 것 같습니다. <br>
(반드시 구분해서 쓰는 것이 좋습니다.)

[Javascript의 undefined는 정확히 무슨 뜻일까? (null vs undefined)](https://siyoon210.tistory.com/148)