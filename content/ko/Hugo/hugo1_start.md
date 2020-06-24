---
title: "1. ubuntu 환경에서 Hugo 시작하기"
date: 2020-06-24T15:44:44+09:00
type: docs
weight: 
description: > 
    hugo + github action 으로 github.io 페이지 만들기
---

- Install Hugo https://gohugo.io/getting-started/installing
- Create Hugo Site https://gohugo.io/getting-started/quick-start/
- Directory Structure https://gohugo.io/getting-started/directory-structure/

## Why Hugo?

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

### __2. github repository 생성__
github에서 repository를 생성하고 clone을 받는다. 
```
git clone 깃헙주소
```
클론을 받지 않고 hugo site를 만든 다음 해당 디렉토리에서 `git remote add origin 레포지토리 주소`를 입력하는 방법도 있다. 순서만 다를 뿐이다. 

### __3. Hugo Site 만들기__ 
clone 받은 github repo이름은 hugo_blog, 생성할 폴더이름은 ceres_blog로 가정하겠다.

1) 먼저 clone 받은 hugo_blog로 이동한다. 
```
cd hugo_blog
```
2) ceres_blog라는 폴더에 hugo site를 제작한다.
```
hugo new site ceres_blog
```
site가 만들어지면 아래와  같은 congratulations 메세지를 볼 수 있다. 

![](https://images.velog.io/images/ceres/post/c6586bc5-ef84-4959-bc4d-2884430e4b2b/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202020-06-19%2015-40-27.png)

### __4. 테마 받기__
테마는 https://themes.gohugo.io/ 여기서 고르면 된다. 각 테마마다 설치방법이 약간씩 다를 수 있다. 각 테마의 github page나 공식문서를 참조하길 바란다.

나는 __Docsy__ 테마를 선택했다. hugo 공식사이트에 예로 나오는 테마는 `ananke` 이다. 만약 hugo 사이트를 처음 만들어 본다면 이 테마로 우선 만들어보는 것이 좋다. 
 
1. hugo site 디렉토리로 이동
`cd ceres_blog`

2. git 초기화 
`git init`

3. 선택한 hugo 테마 github 주소 복사 

    테마의 css를 수정하고 싶다면 테마의 github 주소를 folk한 다음 folk 한 주소를 사용해야한다. 
4. submodule에 테마 추가
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

`hugo new 폴더이름/파일이름.md ` 명령어를 적으면 content 폴더 하위에 폴더가 생성되고 그 하위에 파일이 생성된다. 이 코드는 config.toml이 있는 root directory에서 입력해야 한다. 여기에선 `cd ceres_blog` 상태에서 작성하면 된다. 

아래 코드를 작성하면 `content` 폴더안에> `ko` 폴더가 생성된고> 그 안에 `Hugo` 폴더> 그 안에 `start_hugo` 라는 md 파일이 생성된다. 만약 ko, Hugo 폴더가 있다면 start_hugo 파일만 생성된다. 
```
hugo new ko/Hugo/start_hugo.md
```
만들어진 폴더에 들어가면 나오는 default 내용이다. 제목, 날짜 등으로 본문내용에는 나오지 않는다. 이 뒤부터 마크다운으로 내용을 작성하면 된다. 
```
---
title: "start_hugo"
date: 2019-03-26T08:47:11+01:00
draft: true
---
```
title은 제목, date는 날짜이다. 
draft는 surver를 연결했을때 화면에서 보여주는 것을 결정한다. draft:true 인 경우 서버를 켜도 해당 페이지는 보이지 않는다. 미완성인 페이지나 보여주고 싶지 않은 페이지는 이 코드를 넣으면 된다. 
<br>
- content default 값을 바꾸는 방법: `archetypes`

    root directory 안 (ceres_blog 폴더 내부)> archetypes 폴더에 > default.md 파일이 있다. 
    
    그곳에다 `hugo new 폴더이름/파일이름` 명령어를 작성했을때 만들어지는 default 내용을 설정할 수 있다. 

    만약 archetype 폴더나 default.md 파일이 없다면 똑같은 이름으로 만들어주면 된다.

- 인덱스 페이지: `_index`

    각 디렉토리의 index 페이지를 만들어 줄 수 있다. 사진과 같이 content가 나오는 것이 아니라 하위 디렉토리가 나오는 페이지이다.  이페이지를 만드려면 각 디렉토리에 `_index.md`파일을 만들면 된다.

<img src="https://images.velog.io/images/ceres/post/badeb562-5b55-4380-94e3-c9d0f80f3156/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202020-06-24%2019-55-13.png" width=70%>
    