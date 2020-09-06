---
layout: post
title:  "AI framework Development 4"
date:   2020-09-04
excerpt: "국가별 데이터 모으기"
tag:
- AI
- framework
- blog
- silofox
comments: true
feature: https://github.com/siloFoX/siloFoX.github.io/blob/master/images/help/help.jpg?raw=true
---

# 하..

2020 9월 4일 기준이다.<br>
통계에서 콜롬비아는 제외하기로 했다.

<br>

## OECD 국가 리스트

"Australia", "Austria", "Belgium", "Canada", "Chile", "Colombia", "Czech Republic", "Denmark", "Estonia", "Finland", "France", "Germany", "Greece", "Hungary", "Iceland", "Ireland", "Israel", "Italy", "Japan", "Korea", "Latvia", "Lithuania", "Luxembourg", "Mexico", "Netherlands", "New Zealand", "Norway", "Poland", "Portugal", "Slovak Republic", "Slovenia", "Spain", "Sweden", "Switzerland", "Turkey", "United Kingdom", "United States"

근데 내가 가지고 있는 국가 데이터랑 매칭이 안된다.<br>
체크 해보니까
```
Czech Republic is not verified country.
**Korea and Republic of Korea are Similar.
Korea is not verified country.
Slovak Republic is not verified country.
**United Kingdom and The United Kingdom are Similar.
United Kingdom is not verified country.
**United States and United States of America are Similar.
United States is not verified country.
```

Czechkia, Republic of Korea, Slovakia, The United Kingdom, United States of America 로 고치고 다시 리스트

"Australia", "Austria", "Belgium", "Canada", "Chile", "Colombia", "Czechia", "Denmark", "Estonia", "Finland", "France", "Germany", "Greece", "Hungary", "Iceland", "Ireland", "Israel", "Italy", "Japan", "Latvia", "Lithuania", "Luxembourg", "Mexico", "Netherlands", "New Zealand", "Norway", "Poland", "Portugal", "Republic of Korea", "Slovakia", "Slovenia", "Spain", "Sweden", "Switzerland", "The United Kingdom", "Turkey", "United States of America"

<br>

## OECD 국가 코드

'AU', 
'AT', 
'BE', 
'CA', 
'CL', 
'CO', 
'CZ', 
'DK', 
'EE', 
'FI', 
'FR', 
'DE', 
'GR', 
'HU', 
'IS', 
'IE', 
'IL', 
'IT', 
'JP', 
'LV', 
'LT', 
'LU', 
'MX', 
'NL', 
'NZ', 
'NO', 
'PL', 
'PT', 
'KR', 
'SK', 
'SI', 
'ES', 
'SE', 
'CH', 
'GB', 
'TR', 
'US'

<br>

## OECD 인당 GDP

2019년 기준<br>
일반 GDP : 억 단위이다.<br>
USD 기준이다.

54401.4253,
53902.94904,
55590.13046,
53198.17296,
26915.8365,
몰라(콜롬비아 빼 ㅠㅠㅠ),
29280.78997,
57149.59431,
30296.87726,
45697.55615,
46480.6154,
53637.8016,
27459.1259,
26223.19671,
68005.79368,
50490.47141,
39403.10257,
39189.36576,
38617.46549,
28453.58675,
28913.89776,
68680.52973,
17594.46105,
56552.29043,
44030.74518,
54027.48705,
31969.56803,
26633.71719,
42284.78911,
25452.38663,
40219.62097,
38757.56996,
46695.34986,
66566.72632,
47226.08766,
65835.57764

<br>

## OECD 인구

만 단위이다.

'2499',
'885.9',
'1146',
'3759',
'1873',
'4965',
'580.6',
'551.8',
'132.9',
'551.8',
'6699',
'8302',
'1072',
'977.3',
'36.41',
'490.4',
'888.4',
'6036',
'12650',
'192',
'279.4',
'61.39',
'12620',
'1728',
'488.6',
'543.3',
'3797',
'1028',
'5164',
'545.8',
'208.1',
'4694',
'1023',
'857',
'6665',
'8200',
'32820'

Normalized 한 결과

0.0761426 , 0.02699269, 0.03491773, 0.11453382, 0.05706886,
0.15127971, 0.01769043, 0.01681292, 0.00404936, 0.01681292,
0.20411335, 0.25295551, 0.03266301, 0.02977757, 0.00110938,
0.01494211, 0.02706886, 0.18391225, 0.38543571, 0.00585009,
0.0085131 , 0.00187051, 0.38452163, 0.05265082, 0.01488726,
0.01655393, 0.11569165, 0.03132236, 0.15734308, 0.0166301 ,
0.00634065, 0.14302255, 0.03117002, 0.02611213, 0.20307739,
0.24984765, 1.

<br>

## OECD 인구밀도

제곱킬로미터 당 인구밀도이다.

3, 109, 383, 4, 26, 46, 139, 137, 31, 18, 119,
240, 81, 107, 3, 72, 400, 206, 347, 30,  43, 242,
66, 508, 18, 15, 124, 111, 527, 114, 103, 94,  25,
219, 281, 110, 36

Normalized 한 결과

0.0056926 , 0.20683112, 0.72675522, 0.00759013, 0.04933586,
0.08728653, 0.26375712, 0.25996205, 0.05882353, 0.0341556 ,
0.22580645, 0.45540797, 0.15370019, 0.20303605, 0.0056926 ,
0.13662239, 0.75901328, 0.39089184, 0.65844402, 0.056926  ,
0.08159393, 0.45920304, 0.12523719, 0.96394687, 0.0341556 ,
0.028463  , 0.23529412, 0.21062619, 1.        , 0.21631879,
0.19544592, 0.17836812, 0.04743833, 0.41555977, 0.53320683,
0.20872865, 0.0683112

