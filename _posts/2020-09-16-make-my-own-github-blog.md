---
layout: post
title: "Make my own github blog" 
date: 2020-09-16 
category: read 
excerpt: ""
---



# Make my own github blog

TIL 기록할 깃허브 블로그 만들기

원래 깃허브 블로그 있었는데 그건 theme를 fork한 방식이고

나는 테마 바꾸는 거 좋아하는 변덕쟁이라 그냥 새 레포 파서 만드는 방식으로 처음부터 진행할 것!

[MacOS 개발환경 설정](https://iamcho2.github.io/2020/09/16/macos-dev-env-setting) 을 마친 상태에서 진행함



## Make repository

깃허브에 username.github.io 이름의 repo 만들어주기

Finder github 폴더에 git clone 해주기

```
$ git clone https://github.com/iamcho2/iamcho2.github.io.git
```





## Install Ruby

Jekyll을 설치하기 위해 Ruby를 먼저 설치해줘야 한다.

```
$ brew install rbenv ruby-build
$ rbenv install 2.7.0
```



## Jekyll

### Install

```
$ sudo gem install jekyll bundler
```

### Create 

```
$ jekyll new ./
$ bundle install
```

### Excute 

```
$ bundle exec jekyll serve
```

여기까지 하면 http://127.0.0.1:4000/ 여기서 생성된 사이트를 확인할 수 있음



## Apply Theme

### Themes

- http://jekyllthemes.org/
- https://jekyllthemes.io/free
- http://themes.jekyllrc.org/
- https://github.com/topics/jekyll-theme

테마 골랐으면 Download ZIP 후 ZIP 내 폴더들, 파일들을 내 레포 폴더로 옮겨줌 (겹치는 건 덮어쓰기)



### Install plugins

```
$ bundle install
```

테마에서 요구하는 플러그인들을 설치해줌

```$
$ bundle exec jekyll serve
```

잘 적용됐는지 확인



### Modify _config.yml

_config.yml 파일을 수정해줘야하는데

저 파일은 테마마다 내용이 달라서 테마에 맞춰서 알아서 수정할 부분 수정하면 됨



## Publishing

add, commit, push 해준다

```
$ git add .
$ git commit -m "blah blah"
$ git push origin master
```



