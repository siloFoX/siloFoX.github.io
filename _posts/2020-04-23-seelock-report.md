---
layout: post
title:  "2020-04-23 SeeLock bugs & requirements"
date:   2020-04-21
excerpt: "SeeLock 버그와 요구사항"
tag:
- mean 
- mean stack
- mongodb
- mongo
- node
- node.js
- handsontable
- axios
- express
- blog
- project
- seelock
- silofox
comments: true
lastmod : 2020-04-21
sitemap : 
  changefreq : never
  priority : 1.0
feature: https://github.com/siloFoX/siloFoX.github.io/blob/master/images/seelock/seelock-feature.jpg?raw=true
---

## 현재 요구사항 정리

    1. 검색 시에 Start Date와 End Date가 같은 경우에는 검색이 안 됩니다. 
    즉, 단 하루만 보는 것이 불가능합니다. 
    연구노트의 기본 원칙이 하루동안 실험한 내용을 적는 것이 기본이었기 때문에 수정이 되었으면 합니다.

    2. Search를 누르면 표들이 정렬된 창이 나타나고, 몇 초 후에 꺼지면서 해당 페이지가 png 파일로 저장이 됩니다. 
    pdf라고 말씀하셨던거같은데, png로 저장이 되네요!

    3. 비고란에 적은 부분이 print 버전의 하단에 반영이 되지 않습니다.

    4. 예전에 업로드한 데이터가 샘플준비에서만 확인이 되고, 
    다른 단계에서는 보이지가 않습니다. 예전 데이터는 날아간 것인가요?

    5. 현재 Process가 아니라, 새로운 실험에 대한 Process에 적용하게 되는 경우, 구축하는데 얼마나 걸릴까요?
    또한, 현재 사용하고 있는 연구노트와 병행하며 함께 유지가 가능한가요?

    + 사진 파일을 연구노트에 출력
    + 기한은 이번주 내
    + 업데이트 모드에서 기판번호 순서대로 sorting 해서 나왔으면 좋겠다

## 그에 대한 답변

    1. 내가 원래 막아놓았는데 풀어놓으면 된다. (그 때는 무슨 생각으로 막았을까 ㅠㅠ)

    2. 페이지도 일부러 꺼지게 해놨는데 풀어놔야겠다.. 그리고 pdf로 저장하는거는 제대로 될 지는 모르겠네

    3. 비고가 출력 안되는 버그가 있네요.. 찾아봅시다.

    4. 이게 제일 심각한 문제 MongoDB가 초기화 되는 버그가 있는 것 같아요. 백업하는 코드를 짜놔서 그나마 다행이긴 한데 

    5. 더 큰 범위의 Wrapping이 필요하다는 것. 
    근데 이거는 윗단계의 개발이 필요한 부분이라 구버전에서는 안 하기로 했었는데
    해야하나...?

## 차근차근 하나씩 해결해보기

우선 branch develop으로 이동한다.<br>
release branch 는 IP 가 localhost 가 아니어서 test 환경에 맞지 않기 때문이다.

1. start Date 와 End Date 같아도 처리될 수 있게
public/js/search.js
```js
function isDataGood() {
    
    if(isNameGood() && isStartDateGood() && isEndDateGood()) {
        
        if(transformToDate(startInput.value).getTime() < transformToDate(endInput.value).getTime())
            return true;
        else 
            alert("Start Date is over than End Date! Please check the dates.")
    }
    else
        return false;
}
```
이 부분에서 
```javascript
if(transformToDate(startInput.value).getTime() <= transformToDate(endInput.value).getTime())
```
이렇게만 바꿔주면 일단 start와 end의 값이 같아도 true 값이 return 되겠지<br>
일단 하나 해결

2. 페이지 안 닫히게 하기 & pdf로 저장하기
public/js/print.js<br>
페이지 안 닫히게 하는건 저기 주석처리한 부분을 삭제하면 끝~
```js
function getPicture () {

        html2canvas(document.body).then(function(canvas) {

            let imgData = canvas.toDataURL("image/png");
            let link = document.createElement("a")

            link.download = global_info[0]["실험자명"] +  " 연구원 연구노트 (" + global_info[0]["실험날짜"] + "-" + global_info[global_info.length - 1]["실험날짜"] + ")"
            link.href = imgData
            document.body.appendChild(link)
            link.click()

            // 여기여기
            window.close() // 이 부분을 삭제한다.
            // 여기여기
        })
}
```
근데 pdf나 png나 상관없지 않나.. 왜 pdf로 꼭 해달라는 것일까 일단 그래도 노력해보자.<br>
우선 views/print.js <head>에다가 script 태그를 추가한다.
```html
<script src="https://unpkg.com/jspdf@latest/dist/jspdf.min.js"></script>
```
그리고
```js
function getPicture () {

        html2canvas(document.body).then(function(canvas) {

            let imgData = canvas.toDataURL("image/pdf");
            let link = document.createElement("a")

            link.download = global_info[0]["실험자명"] +  " 연구원 연구노트 (" + global_info[0]["실험날짜"] + "-" + 
                            global_info[global_info.length - 1]["실험날짜"] + ")"
            link.href = imgData
            document.body.appendChild(link)
            link.click()
        })
}
```
이 코드를 고쳐야 하는데 현재 작동방식에 대해 간단히 말해보자면,<br><br>
html2canvas라는 tool에서 document의 body를 argument로 가져왔다.<br>
canvas의 정보를 DataURL 이라는 형태로 바꿔서 imgData에 전달한다.<br><br>
document 안에 <a>태그를 하나 생성하는 코드를 link 객체에 할당한다. <br>
link.download에 파일이름을 넣는다.<br>
link.href에 이미지를 넣는다.<br>
document.body에 link를 넣는다.<br>
link를 실행시킨다.(그럼 다운로드가 된다.)<br><br><br>
이것을 jsPDF를 이용해서 pdf 형식으로 다운로드 하겠다.
```js
function getPicture () {

    html2canvas(document.body).then(function(canvas) {

        let imgData = canvas.toDataURL("image/png");

        let file_name = global_info[0]["실험자명"] +  " 연구원 연구노트 (" + global_info[0]["실험날짜"] + "-" + 
                        global_info[global_info.length - 1]["실험날짜"] + ").pdf"
        let doc = new jsPDF({orientation : 'landscape'})

        let imgProps = doc.getImageProperties(imgData)

        var width = doc.internal.pageSize.getWidth();
        var height = (imgProps.height * width) / imgProps.width;
        doc.addImage(imgData, 'PNG', 0, 0, width, height);

        doc.save(file_name)
    })
}
```
그러나 작동하지 않았다.<br>
url/print 에서 chrome 개발자 도구를 키고 console 에 찍힌 오류를 찾아보았다.
```s
Uncaught (in promise) TypeError: get_URL(...).createObjectURL is not a function
    at new FileSaver (jspdf.debug.js:17665)
    at saveAs (jspdf.debug.js:17681)
    at Object.API.save (jspdf.debug.js:3648)
    at print.js:121
```
다행히 jspdf 를 debug 모드로 link 시켜놓아서 찾는데 원인 코드는 금방 찾았다.<br>
그러나 구글에 아무리 쳐봐도 모르겠었는데(이렇게 말은 하지만 이 문제로 1주일 이상 고생했다.)...<br>
문제가 되는 부분은
```js
if (!object_url) {
    object_url = get_URL().createObjectURL(blob);
}
```
이 부분인데 createObjectURL 이라는 함수가 메소드로 호출되지 않는 것이 문제였다.<br>
그래서 get_URL에서 무슨 객체를 반환하는지 살펴볼 필요가 있었다.
```js
get_URL = function () {
    return view.URL || view.webkitURL || view;
}
```
그래도 무슨 문제인지 잘 감이 안 왔다.<br>
그래서 조금 더 찾아보기로 했다. view 라는 객체가 무엇인지 알기 위해서
```js
var saveAs = saveAs || function (view) { ... }
```
이렇게 되어 있었다. 이 꼴은 <br>
var function_name = function_name || function (parm, ...) { ... } 대충 이런 꼴인데 <br>
[이 글](https://stackoverflow.com/questions/7069302/what-does-the-javascript-expression-a-a-function-mean)에 잘 설명되어 있었다.<br>
간단하게 말하자면 function_name 이 다른 곳에 선언되어 있으면 그것을 쓰고<br>
아니면 || 뒤에 있는 것을 넣겠다는 뜻이다.<br>
이것은 || 연산자의 특성 때문인데, || 연산자는 앞 뒤에 있는 값 중에서 하나를 반환해주는 연산자이다.<br>
(if condition 안에서는 자동으로 boolean type casting 이 되기 때문에 잘 모르는 사실이다.)<br><br>
아무튼 stackoverflow의 [어떤 글](https://github.com/eligrey/FileSaver.js/issues/143)을 보고 해결 하게 되었다.<br>
siva3378 : <b>It happend because, My app has a global object "URL" which is actually overriding window.URL object.</b><br>
해석하자면, 전역변수인 URL이 선언되어 있어서 overriding 되었다는 것이다. <br>
그래서 코드를 더 뜯어보니 global.URL 을 사용한 흔적이 있었다.<br>
겨우겨우 찾았다...

3. 비고가 출력 안되는 버그가 있다.
아직 어디서 그런 버그가 발생하는지 확인하지 못 했다.. 어딜까??

4. DB가 초기화 되는 버그
이건 알고보니 랜섬웨어였다... 이런 젠장 나쁜 놈들<br>
```s
All your data is a backed up.
You must pay 0.015 BTC to 1JNL8EyXwY3tAf1qtxCxCJwdct1UrT5LMJ 48 hours for recover it.
After 48 hours expiration we will leaked and exposed all your data.
In case of refusal to pay, we will contact the General Data Protection Regulation,
GDPR and notify them that you store user data in an open form and is not safe.
Under the rules of the law, you face a heavy fine or arrest and your base dump will be dropped from our server!
You can buy bitcoin here, does not take much time to buy https://localbitcoins.com with this guide
https://localbitcoins.com/guides/how-to-buy-bitcoins After paying write to me in the mail with your DB IP: get_base@tuta.io
```
DB 를 날려버리고 돈 내놓으라고 써놨다.<br>
찾아보니 몽고DB의 취약점을 이용했다고 한다.<br>
근데 뭐 내가 27017 기본 포트를 사용하기도 했고 auth 설정도 안 걸어놨으니<br>
그거 걸고 버전 업그레이드하면 될 것 같다.

5. Process 추가와 새로운 공정 넣기 : 지금은 무리

6. 사진 넣는건 원래 하기로 했으므로 한다.
가로 픽셀 계산<br><br>
픽셀 : 2480<br>
가로 : 21<br>
1 cm 당 픽셀 : 118.09<br>
8 cm 픽셀 : 945<br>
이 기준대로 하니까 제대로 맞지를 않았다. 화소랑 매치가 안되는 것 같았다.<br><br><br>
일단 지금 할게 많아서 소스만 올리고 리뷰는 나중에<br>
```js
function getPicture () {
    
        let heads = document.getElementsByClassName("wtHider")
        let max_length = 0

        for (let idx = 0; idx < heads.length; idx++) {
            
            let width_num = heads[idx].style.width.slice(0, -2)
            width_num = parseInt(width_num)

            if (width_num > max_length)
                max_length = width_num
        }
        
        memo_form.style.width = max_length + 100 + "px"
        max_length += 200
        image_div_class.style.width = max_length + "px"
        image_div_class.style.height = max_length * 29 / 21 + "px"

        document.body.style.zoom = 0.6

        html2canvas(image_div_class).then(function(canvas) {

            let file_name = global_info[0]["실험자명"] +  " 연구원 연구노트 (" + global_info[0]["실험날짜"] + "-" + global_info[global_info.length - 1]["실험날짜"] + ").png"

            canvas.toBlob(function (blob) {
                var url = window.URL || window.webkitURL;
                var imgSrc = url.createObjectURL(blob);
                var img = new Image();
                img.src = imgSrc;
                img.onload = function () {
                    var pdf = new jsPDF('p', 'px', [img.height, img.width]);

                    pdf.addImage(img, 0, 0, img.width - 350, img.height - 350);
                    pdf.save(file_name + '.pdf');
                }; 
            })
        })
}
```

7. 이것도 기준에 맞춰서 sort 하는거니까 충분히 할 수 있을 것 같다.