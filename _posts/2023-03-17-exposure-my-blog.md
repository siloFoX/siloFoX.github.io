---
layout: post
title:  "Exposure my blog"
date:   2023-03-17
excerpt: "검색엔진(구글)에 내 블로그를 노출시켜보자"
tag:
- coding
- sitemap
- crawler
- user-agent
- robots.txt
- rss
- rss feed
- git
- github
- blog
- silofox
comments: true
lastmod : 2023-03-26
sitemap : 
  changefreq : never
  priority : 1.0
---

키보드가 승인되고 나서 이제 핸드폰으로 자유롭게 코딩을 할 수 있게 되었다. 일과시간동안 개인정비시간에 무엇을 할 지 고민하였는데, 일단 블로그를 Google 검색엔진에 노출하는 작업을 하지 않았던 것이 생각났다. 그래서 손가락 워밍업도 할 겸 sitemap tree 를 작성해보기로 했다.<br><br>

구글 같은 검색엔진에서는 crawler 를 이용하여 검색이 가능하게 site 들을 추적한다. 그래서 먼저 내 Site 를 추적하게 쉽게 crawler 가 대문에서 읽을 수 있는 룰을 고지해줘야한다. 이미 Github pages 를 통해서 대문은 찾을 수 있기 때문이다.<br>
그래서 robots.txt 를 먼저 Root dir 에 생성하기로 했다. <br>

<b>/robots.txt</b>

```txt
User-agent : *
Allow : /
User-agent : BadBot
Disallow : /

Sitemap : http://www.silofox.github.io/
```

User-agent 를 전부 허용해놓는 이유는 Github pages 에서 load blancing 을 해줄 것이란 믿음 때문이고, 그렇지 않은 경우는 자신이 직접 Bot 종류를 추가해주고 Disallow 로 막아주면 된다.<br><br>

다음은 rss feed 를 구성해준다. Root dir 에 만들어준다. 지금 환경상 컴퓨터를 사용할 수 없기 때문에 (물론 핸드폰이 컴퓨터이긴 하지만 일반 데스크탑 환경에서 하는 테스트 들을 편하게 할 수 없다.) jekyll plugin 들을 사용하지 않고 직접 어느 정도 작성해주기로 했다.<br>

<b>/feed.xml</b>

```HTML
---
layout: null
---
<?xml version="1.0" encoding="UTF-8"?>
<rss version="2.0" xmlns:atom="http://www.w3.org/2005/Atom">
  <channel>
    <title>{{ site.title | xml_escape }}</title>
    <description>{{ site.description | xml_escape }}</description>
    <link>{{ site.url }}{{ site.baseurl }}/</link>
    <atom:link href="{{ "/feed.xml" | prepend: site.baseurl | prepend: site.url }}" rel="self" type="application/rss+xml"/>
    <pubDate>{{ site.time | date_to_rfc822 }}</pubDate>
    <lastBuildDate>{{ site.time | date_to_rfc822 }}</lastBuildDate>
    <generator>Jekyll v{{ jekyll.version }}</generator>
    {% for post in site.posts limit:30 %}
      <item>
        <title>{{ post.title | xml_escape }}</title>
        <description>{{ post.content | xml_escape }}</description>
        <pubDate>{{ post.date | date_to_rfc822 }}</pubDate>
        <link>{{ post.url | prepend: site.baseurl | prepend: site.url }}</link>
        <guid isPermaLink="true">{{ post.url | prepend: site.baseurl | prepend: site.url }}</guid>
        {% for tag in post.tags %}
        <category>{{ tag | xml_escape }}</category>
        {% endfor %}
        {% for cat in post.categories %}
        <category>{{ cat | xml_escape }}</category>
        {% endfor %}
      </item>
    {% endfor %}
  </channel>
</rss>
```

다음은 sitemap.xml 을 구성해줄 것이다.<br>

간단하게 HTML 과 XML 의 차이를 서술하겠다. XML 이란 Extensible Markup Language 로 겉보기에는 HTML 과 비슷할 수 있으나, HTML 은 데이터를 표시하는 역할이고 XML 은 데이터를 저장하고 전송하는 역할을 하게 된다. 둘 다 Markup Language 이기 때문에 tag 를 사용하여 데이터의 위치를 표기하며 XML 만 대소문자를 구분한다.<br>
여기서도 마찬가지로 Liquid 의 template 문법을 사용할 수 있다. Root dir 에 /sitemap.xml 을 만든다.<br>

<b> /sitemap.xml </b>

```HTML
---
layout: null
---
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.sitemaps.org/schemas/sitemap/0.9 http://www.sitemaps.org/schemas/sitemap/0.9/sitemap.xsd" xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  {% for post in site.posts %}
    <url>
      <loc>{{ site.url }}{{ post.url }}</loc>
      {% if post.lastmod == null %}
        <lastmod>{{ post.date | date_to_xmlschema }}</lastmod>
      {% else %}
        <lastmod>{{ post.lastmod | date_to_xmlschema }}</lastmod>
      {% endif %}

      {% if post.sitemap.changefreq == null %}
        <changefreq>weekly</changefreq>
      {% else %}
        <changefreq>{{ post.sitemap.changefreq }}</changefreq>
      {% endif %}

      {% if post.sitemap.priority == null %}
          <priority>0.5</priority>
      {% else %}
        <priority>{{ post.sitemap.priority }}</priority>
      {% endif %}

    </url>
  {% endfor %}
</urlset>
```

lastmod, changefreq, priority 에 해당하는 부분은 어차피 구글 정책에 따라 의미없는 정보라고 판단되면 무시될 것이다.<br>

그래도 신경쓰인다면 XML 에서 Sitemap 형식을 찾아보기 바란다.<br>
lastmod 는 Datetime 형식의 YYYY-MM-DD<br>
changefreq 는
- always
- hourly
- daily
- weekly
- monthly
- yearly
- never
priority 는 0.0 ~ 1.0 사이의 값인데 우리가 임의로 변경하는 것은 그닥 의미가 없을 것 같다.

마지막으로 md file 전반에 lastmod, changefreq, priority 변수를 추가할텐데 이게 Liquid 랑 HTML 으로 되어있는 템플릿 부분을 수정할 필요가 없을지 모르겠어서 일단 추가만 해두고 추후에 HTML 에 수정해볼 생각이다.<br><br>

최근에 친구가 같이 어플을 만들자고 하여서 어차피 할 것도 없었는데 재밌을 것 같아서 만들어볼 생각이다.<br>
여러가지 생각할 것들이 많다. 조만간 또 올려야겠다. 시장조사, 기획, Flutter, 개발툴 정도..<br>