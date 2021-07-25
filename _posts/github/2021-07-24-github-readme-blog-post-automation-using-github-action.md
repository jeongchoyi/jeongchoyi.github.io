---
layout: post
title: "Github Action을 이용한 README 블로그 포스트 automation" 
date: 2021-07-24
category: read 
excerpt: ""

---

# Github Action을 이용한 블로그 포스트 automation

![image](https://user-images.githubusercontent.com/28949235/126894324-74bbf446-694f-449d-a137-8198ffaf6dbf.png)

깃허브에서 본인의 username과 같은 이름의 repo를 만들면 

![image](https://user-images.githubusercontent.com/28949235/126894347-9a56bec1-19fe-4e26-b7a0-23b8103352cb.png)

요렇게 메인에 README가 뜨는데, 요 리드미에 최근에 작성한 블로그 포스팅들을 자동으로 올리고 싶었다.  
이는 Github Action을 사용하면 가능하다고 한다.

### Github Action?

Github Action은 Github 저장소를 기반으로 소프트웨어 개발 Workflow를 자동화 할 수 있는 도구이다.  
Github에서 직접 제공하는 CI/CD 도구 같은 것인데,  
이 변수에 이 값이 잘 들어가는지, 이 파일의 포맷이 맞는 포맷인지 등을 검사해준다.

build, test, package, release, deploy 등의 이벤트를 기반으로 직접 원하는 workflow를 만들 수 있다고 한다.  
Public repo에서는 무료라고 한다 ~~! (제한이 조금 있긴 함)

우리는 github blog post workflow를 만들면 되는 것 !  
Action은 직접 만들거나, 마켓스페이스에서 가져와서 라이브러리 같은 개념으로 사용할 수 있다.

![image](https://user-images.githubusercontent.com/28949235/126894526-e4158295-d3f0-4062-87d4-4355cbb7981e.png)

우리가 코드를 하나하나 짤 필요는 없고, 고수분들이 만들어주신 것을 감사히 사용하면 된다 ( ◠‿◠ )   
링크는 [여기](https://github.com/gautamkrishnar/blog-post-workflow)

사용법을 알아보도록 하자

### README.md에 주석 추가하기

```
<!-- BLOG-POST-LIST:START -->
<!-- BLOG-POST-LIST:END -->
```

readme 파일에 위 구문을 추가해준다.  
workflow가 작동하면서 저 사이를 블로그 포스팅 목록으로 대체해줄거라, 원하는 부분에 넣으면 된다

### .github 폴더랑 workflow 폴더 만들기

![image](https://user-images.githubusercontent.com/28949235/126895057-9201c8d6-b127-40be-b27e-1c8e35995344.png)![image](https://user-images.githubusercontent.com/28949235/126895059-93020ab0-afa5-40db-b714-4fa61526051b.png)

Finder에서 하려고 하면 이런 식으로 오류가 나기 때문에... VSCode에서 한방에 해줬다.

![image](https://user-images.githubusercontent.com/28949235/126895289-21bbdf77-b017-464f-8dc8-fcbcb0755b7a.png)

`.github/workflows/blog-post-workflow.yml` 요렇게 만들어주면 위와 같이 만들어진다.

그리고 빈 yml파일에 아래 코드를 붙여넣기 해 준다.

```yml
name: Latest blog post workflow
on:
  schedule: # Run workflow automatically
    - cron: '0 * * * *' # Runs every hour, on the hour
  workflow_dispatch: # Run workflow manually (without waiting for the cron to be called), through the Github Actions Workflow page directly
jobs:
  update-readme-with-blog:
    name: Update this repo's README with latest blog posts
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: gautamkrishnar/blog-post-workflow@master
        with:
          feed_list: "내 블로그 피드 주소"
```

블로그가 여러개라면 feed_list에서 comma로 구분해서 작성하면 된다고 한다.  
그리고 commit, push 하면 된다.  
근데, 내 블로그는 jekyll로 만든 블로그라서, rss feed가 저 repo의 readme에 없었다.

### jekyll blog rss feed

![image](https://user-images.githubusercontent.com/28949235/126895640-ac7c73b7-458c-4272-8980-fcf56ed1f8da.png)

예전에 블로그 검색 노출을 위해 feed.xml을 생성해놔서 추가적인 작업은 필요하지 않았지만,  
혹시 몰라 feed.xml의 내용을 아래에 ,,

```
---
---
<?xml version="1.0" encoding="UTF-8"?>
<rss version="2.0" xmlns:atom="http://www.w3.org/2005/Atom" xmlns:dc="http://purl.org/dc/elements/1.1/">
    <channel>
        <title>{{ site.name | xml_escape }}</title>
        <description>{% if site.description %}{{ site.description | xml_escape }}{% endif %}</description>
        <link>{{ site.url }}</link>
        <atom:link href="{{ site.url }}/{{ page.path }}" rel="self" type="application/rss+xml" />
        <lastBuildDate>{% for post in site.posts limit:1 %}{{ post.date | date_to_rfc822 }}{% endfor %}</lastBuildDate>
        {% for post in site.posts limit:10 %}
            <item>
                <title>{{ post.title | xml_escape }}</title>
                {% if post.author.name %}
                    <dc:creator>{{ post.author.name | xml_escape }}</dc:creator>
                {% endif %}
                {% if post.excerpt %}
                    <description>{{ post.excerpt | xml_escape }}</description>
                {% else %}
                    <description>{{ post.content | xml_escape }}</description>
                {% endif %}
                <pubDate>{{ post.date | date_to_rfc822 }}</pubDate>
                <link>{{ site.url }}{{ post.url }}</link>
                <guid isPermaLink="true">{{ site.url }}{{ post.url }}</guid>
            </item>
        {% endfor %}
    </channel>
</rss>
```

그리고 블로그 피드 주소를 적는 곳에 `https://iamcho2.github.io/feed.xml` 을 적어주면 완성!

### 확인하기, 수동으로 trigger하기

![image](https://user-images.githubusercontent.com/28949235/126895869-41457a3e-aa50-4bf6-ab07-229bdf49d126.png)

그리고 Action에 가보면 내가 추가한 workflow가 떠 있지만, 아직 실행된 워크플로우는 없다.  
수동으로 실행해보자

![image](https://user-images.githubusercontent.com/28949235/126895939-47c9a433-ae6a-4154-9c1b-09cd93f74ebf.png)

워크플로우를 클릭하면, 

![image](https://user-images.githubusercontent.com/28949235/126895980-e3568fb1-aae2-4a70-bbc2-74cfb8c4c3a6.png)

이렇게 수동으로 워크플로우를 실행시킬 수 있다.  
이는 우리 Action workflow yml파일에

```
  workflow_dispatch: 
```

구문이 있기 때문!

![image](https://user-images.githubusercontent.com/28949235/126896021-132b5cd0-e8b7-409a-ba2d-3365f6fab8d4.png)

그럼 요렇게 잘 돌아가고 있는 것을 확인할 수 있다.

### 실행 결과

![image](https://user-images.githubusercontent.com/28949235/126896032-4a16c271-1aeb-4432-9ce4-42eb1b9270f8.png)

그럼 요로코롬 최신 post 5개가 나오는 것을 확인할 수 있다! 야호 ~~

### 더 많은 action들

이외에 다른 유용한 Github Action 모음은 [여기](https://about.codecov.io/blog/discovering-the-most-popular-and-most-used-github-actions/)에서 확인할 수 있다.
