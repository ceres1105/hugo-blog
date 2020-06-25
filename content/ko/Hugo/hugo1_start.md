---
title: "1. ubuntu 환경에서 Hugo 시작하기"
date: 2020-06-24T15:44:44+09:00
type: docs
weight: 1
description: > 
    hugo + github action 으로 github.io 페이지 만들기
---


> hugo는 Go 언어로 만들어진 정적 사이트 생성기이다. </br>
빌드시간이 빠르고 안정적인 특징을 갖고 있다.  

 

## Hugo 블로그 만들기
Hugo 공식문서의 [Quick Start](https://gohugo.io/getting-started/quick-start/)를 참조하였다.


### __1. Hugo 설치하기__
사용하는 OS 마다 설치 코드가 다르다.  설치는 [이곳](https://gohugo.io/getting-started/installing)을 참조하였다.

```
// ubutu 환경
sudo apt-get install hugo
``` 

설치 후에는 설치가 잘 되어있는지 확인한다. 아래의 코드를 입력했을 때 version 정보가 나오면 설치가 된 것이다.

```
hugo version
```
(Mac은  `brew install hugo` 명령어로 설치하면 된다. 당연히 brew가 미리 설치되어있어야 한다.)


### __2. Hugo Site 만들기__ 
clone 받은 github repo이름은 hugo_blog, 생성할 저장소 이름은 ceres_blog로 가정하겠다.

1) 먼저 clone 받은 hugo_blog로 이동한다. 
```
cd hugo_blog
```
2) hugo site를 제작한다. ceres_blog 저장소가 생성되고 그 내부에 hugo site의 컨텐츠들이 생성된것을 볼 수 있다.
```
hugo new site ceres_blog
```
site가 만들어지면 아래와  같은 congratulations 메세지를 볼 수 있다. 

![](https://images.velog.io/images/ceres/post/c6586bc5-ef84-4959-bc4d-2884430e4b2b/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202020-06-19%2015-40-27.png)

### __3. github repository와 연결__
github repository를 먼저 생성한 후 reomote로 등록한다. `git remote add origin 레포지토리 주소` 명령어를 사용한다.  
```
git init
git remote add origin https://github.com/ceres1105/blog_2
```
 cf) repository를 생성한 후 `git clone 레포지토리 주소` 명령어로 clone 받고, clone 받은 디렉토리에서 2번 과정 (Hugo Site 만들기)를 진행해도 된다. 이 경우에는 remote로 따로 등록을 하지 않아도 된다. 

### __4. 테마 설치하기__
테마는 [이곳](https://themes.gohugo.io/)에서 선택하면 된다. 각 테마마다 설치방법이 약간씩 다를 수 있다. 각 테마의 github page나 공식문서를 참조하길 바란다.

나는 __Docsy__ 테마를 선택했다. hugo 공식사이트에 예로 나오는 테마는 `ananke` 이다. 

hugo 사이트를 처음 만들어 본다면 우선 `ananke` 테마로 만드는 것을 추천한다. hugo 공식문서 quick start만 그대로 따라하면 화면이 잘 나올 것이다.  

만약 Docsy 테마를 사용할 예정이라면 docsy theme에 대해 작성한 글에서 설치부분을 참고하여 설치하는 걸 추천한다. 
 
1. hugo site 디렉토리로 이동
`cd ceres_blog`

2. git 초기화 
`git init`

3. 선택한 hugo 테마 github 주소 복사 
    테마의 css를 수정하고 싶다면 테마의 github 주소를 folk한 다음 fork 한 주소를 사용해야한다.

4. submodule로 테마 추가
테마를 clone 해서 사용해도 된다. submodule을 사용하는 이유는 엡데이트 된 테마를 쉽게 가지고 올 수 있기 때문이다. 
`git submodule add 깃헙주소 themes/테마이름`

5. config.toml 파일에 테마 내용 추가
`theme = "테마이름"' >> config.toml`

6. 로컬로 구축시 필요한 코드 추가
`git submodule update --init --recursive`

코드를 정리하면 아래와 같다.
```
cd ceres_blog
git init
git submodule add https://github.com/ceres1105/docsy.git themes/ananke
echo 'theme = "ananke"' >> config.toml
git submodule update --init --recursive
```

### __5. Contents 만들기__

`hugo new 폴더이름/파일이름.md` 명령어를 입력하면 content 폴더 하위에 폴더가 생성되고 그 하위에 파일이 생성된다.

 이 명령어는 config.toml이 있는 `root directory`에서 입력해야 한다. 여기에선 `cd ceres_blog` 상태에서 작성하면 된다. 

아래 코드를 작성하면 `content` 폴더안에> `ko` 폴더가 생성된고> 그 안에 `Hugo` 폴더> 그 안에 `start_hugo` 라는 md 파일이 생성된다. 

만약 ko, Hugo 폴더가 있다면 start_hugo 파일만 생성된다. 
```
hugo new ko/Hugo/start_hugo.md
```
만들어진 폴더에 들어가면 나오는 default 내용이다. 이것을 front matter 라고 한다.  

제목, 날짜 등으로 본문 내용에는 영향을 주지 않는다. front matter 아래에 마크다운으로 본문 내용을 작성하면 된다. 
```
---
title: "start_hugo"
date: 2019-03-26T08:47:11+01:00
draft: true
---
```
- title은 제목, date는 날짜이다. 
- draft는 surver를 연결했을때 화면에서 보여주는 것을 결정한다. draft:true 인 경우 서버를 켜도 해당 페이지는 보이지 않는다. 
- 미완성인 페이지나 보여주고 싶지 않은 페이지는 이 코드를 넣으면 된다. 
<br>
- default 내용 바꾸는 방법: `archetypes`

    root directory 안 (ceres_blog 폴더 내부)> archetypes 폴더에 > default.md 파일이 있다. 
    
    그곳에다 `hugo new 폴더이름/파일이름` 명령어를 작성했을때 만들어지는 default 내용을 수정 할 수 있다. 

    만약 archetype 폴더나 default.md 파일이 없다면 똑같은 이름으로 만들어주면 된다.


### __6. Hugo Server 시작 (로컬서버 연결하기)__
`hugo server -D` 를 입력하고  http://localhost:1313/ 이 주소로 들어가서 내용이 잘 작성되었는지 확인하면 된다. 

- D 는 draft:true인 항목 (=모든 항목)을 로컬서버에서 보여준다는 뜻이다.
- D 가 없이 `hugo server` 명령어만 입력하면 draft: false인 항목만 로컬서버에서 보여준다.

```
//server에 연결이 될 때 터미널에 나오는 화면이다. 
▶ hugo server -D

                   | EN
+------------------+----+
  Pages            | 10
  Paginator pages  |  0
  Non-page files   |  0
  Static files     |  3
  Processed images |  0
  Aliases          |  1
  Sitemaps         |  1
  Cleaned          |  0

Total in 11 ms
Watching for changes in /Users/bep/quickstart/{content,data,layouts,static,themes}
Watching for config changes in /Users/bep/quickstart/config.toml
Environment: "development"
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```
### __7. 사이트 구성__
`config.toml` 파일을 열면 보이는 화면이다. 
```
baseURL = "https://ceres1105.github.io/blog_2/"
languageCode = "en-us"
title = "ceres_blog"
theme = "docsy"
```
title을 설정해준다. baseURL은 URL 주소가 있으면 입력하고 로컬로만 사용하면 바꾸지 않아도 된다. 

나는 github page 주소를 넣었다. 

config.toml 에는 계속해서 넣을 내용이 생길 것이다. 


### __8. Build__
간단하다. `hugo -D` 명령어만 입력해주면 된다. 
build 결과는 public directory에 생성된다. 

__주의!__ 만약 github action을 사용해서 build 할 것이라면 이 과정을 하지 않아야 한다.

만약 `hugo -D`명령어로 build 했다면 github action 전에 public 폴더를 지워야 한다.

그렇지 않으면 변경한 내용이 deploy 에 반영되지 않을 것이다. 

(경험담이다..분명 css와 layout을 변경했는데 배포된 페이지를 보면 전혀 반영이 되어있지 않아 삽질을 했다..public 폴더를 지우니 바로 반영이 되는 걸 볼 수 있었다.)

다음 포스팅에는 github action을 사용하여 github page로 배포하는 것을 알아볼 것이다. 