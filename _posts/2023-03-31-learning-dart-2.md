---
layout: post
title:  "Learning Dart by Google 2"
date:   2023-03-31
excerpt: "Dart OOP"
tag:
- coding
- dart
- google
- flutter
- oop
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

dart oop 에 대한 얘기를 해보려 한다.<br>

class 내에서 attribute 랑 겹치는 variable name 이 없을 때 this 생략가능

named constructor 라는 것이 있다.<br>
예시 : (fromMap 같은 constructor name 은 내가 정할 수 있다.)
```dart
className.fromMap(Map<String, dynamic> map)
 : this._att1 = map["key1"],
   this._att2 = map["key2"];
```

private 가 있긴 한데.. class 밖에서 접근 불가능한게 아니고, 같은 파일 내에서만 접근 가능하게 만들어주는 것이란다. 아마도 class 파일을 따로 분리하라는 것 같다. _variableName 이런 식으로 만들면 된다.<br>

getter/setter 는 method 앞에 get/set 을 추가로 붙이면 된다. 요즘은 OOP 에서 immutable variable 을 쓰는 추세이므로 setter 는 거의 사용하지 않는다.<br>

상속은 extends<br>

@override 생략 가능하지만 가독성을 위해 쓰자. (남 코드 볼 때 주의해야할 듯)<br>

interface keyword 는 딱히 없다. 그냥 class 를 만들고 implements 로 적용하면 된다. 그럼 그냥 알아서 기능을 재정의 하도록 강제해준다.<br>

Mixin 은 특정한 class 에서 원하는 기능을 넣을 수 있는 기능이다. 그 class 를 상속하는 class 에서도 사용이 가능하다.<br>
예시를 보자.
```dart
mixin className1Mixin on className1 {
  void functionName ()  {
    print("${this.att1} hello world");
  }
}

class className2  extends superclassName with className1Mixin {
  className2(
    super.att1,
    super.att2,
  );
}
```
이러면 className2 에서 functionName 을 쓸 수 있는 것이다.<br>

abstrat class, generic class, static variable 사용가능<br>
static global variable 이 사용가능한지 검색해볼 예정<br>

cascade operator 는 spread 랑 비슷해보이는데 그냥 chaining 이다.<br>
InstanceName1..methodName1()..methodName2();<br>
이런 식으로 쓰면 된다.<br>

다음에는 async (비동기) 관련 내용을 포스팅 하겠다.