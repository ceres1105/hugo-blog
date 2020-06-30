---
title: "3. Docsy 테마 사용하기"
date: 2020-06-24T17:27:08+09:00
type: docs
weight: 3
draft: false
description: >
    hugo + github action 으로 github.io 페이지 만들기
---
> 이전 포스트에서 테마를 기본테마인 ananke로 만들었다면 배포된 블로그를 볼 수 있을 것이다.
하지만 다른 theme를 사용한 경우라면 page가 생각한대로 나오지 않을 수 도 있고, 심지어는 페이지 자체가 나오지 않을 수 도 있다. 테마가 다른 경우는 각 테마의 설명을 보고 커스터마이징을 해야한다. 나의 경우 docsy 테마를 사용하였다. 

이 포스팅은 [DocsyDocument](https://www.docsy.dev/docs/)를 참고하여 작성하였다. 공식 문서에는 더욱 다양한 기능들이 있으니 참고해서 필요한 기능을 사용하면 된다.

 이 포스팅에서는 내가 추가한 기능만 정리해 두었다.


## __1. Docsy 테마 설치하기__
참고로 나의 경우 테마의 css를 변경할 것이기 때문에 theme github 주소를 그대로 clone 받지 않고 github 주소를 folk한 후 사용했다. 
설치 방법은 [Docsy사이트](https://www.docsy.dev/docs/getting-started/)를 참조하였다.

### 1) PostCSS 설치
PostCSS를 설치하기 위해서 Node.js가 설치되어 있어야 한다. 전역으로 설치 한 경우 PostCSS5.0.1 이상 버전은 로드되지 않기 때문에 로컬 설치를 사용해야한다. (`autoprefixer` 설치)
```
sudo npm install -D --save autoprefixer
sudo npm install -D --save postcss-cli
```

- PostCss 설치시 발생 에러: Package.json
나의 경우 `This utility will walk you through creating a package.json file. `라는 에러가 계속 발생했다. Package.json 파일을 만들어 주지 않았기 때문이다. 

`git init` 코드를 친 후 프로젝트 내용에 대해 적어주자. 잘 모르겠으면 git init 후 엔터를 연달아 치면 package.json 파일이 만들어져 있는걸 볼 수 있다. 

package.json이 설치 된 후에 다시 PostCss 설치를 하니 잘 작동했다.

### 2) Docsy 테마 submodule에 추가

앞 포스팅과 내용이 같다. hugo site 제작 `hugo new site 폴더명` -> 만든 디렉토리로 이동 `cd 폴더명` -> git 초기화 `git init` -> 서브모듈추가 `git submodule add 깃헙주소 themes/테마이름` -> config.toml 파일에 테마 추가 `echo 'theme = "테마이름"' >> config.toml` -> 로컬로 구축시 필요한 코드 작성`git submodule update --init --recursive`

정리하면 아래와 같다
```
hugo new site ceres_blog
cd ceres_blog
git init
git submodule add 깃헙주소 themes/docsy
echo 'theme = "docsy"' >> config.toml
git submodule update --init --recursive
```
여기까지 한 후 `hugo server`로 로컬페이지를 보면 화면이 나오지 않는 경우도 있다. 이때는 `npm install`을 해주면 된다. docsy theme가 scss를 사용하기 때문에 npm install을 해야 작동이 된다. 

## __2. Docsy 테마 Contents__
[DocsyDocument_Adding_Content](https://www.docsy.dev/docs/adding-content/content/)를 참고하였다.

### 1) archetypes 수정

docsy theme에 맞게 page의 front matter를 수정했다.
```
---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
type: docs
weight: 4
draft: false
description: >
    
---
```
- title의 value인 `{{ replace .Name "-" " " | title }}`는 파일명을 타이틀로 쓴다는 의미이다. 
- draft=false를 기본값으로 지정했다. draft: true 상태로 되어있는 경우, 해당 페이지는 화면에 나오지 않는다. 
만약 draft: true인 상태로 페이지를 화면으로 보고 싶다면 `hugo server -D`를 사용해서 로컬서버에서 보면 된다.
- 페이지 정렬 순서는 “Weight”-> “Date”-> “LinkTitle” -> “FilePath” 순이다. 만약 현재 포스팅을 상단에 위치라고 싶다면 weight: 1 을 입력하면 된다.
- docsy theme는 description 을 적을 수 있어서 archetypes에 추가해 주었다.

### 2) index page 생성
각 디렉토리의 index 페이지를 만들어 줄 수 있다. 각 디렉토리에 `_index.md`파일을 생성하면 된다. 

![](https://images.velog.io/images/ceres/post/12ffa1d0-b0ae-401c-a937-8d1a123221b9/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202020-06-25%2017-54-30.png)

`_index.md` 파일의 내용이다. front matter 부분만 작성해주면 된다. 
```
---
title: "Hugo로 블로그 만들기"
linkTitle: "Hugo로 블로그 만들기"
type: docs
weight: 4
description: >
    hugo + github action 으로 github.io 페이지 만들기
---

```
결과물을 보면 페이지 content에 본문이 나오는 것이 아니라 해당 디렉토리에 있는 파일 목록이 나온다.

<img src="https://images.velog.io/images/ceres/post/badeb562-5b55-4380-94e3-c9d0f80f3156/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202020-06-24%2019-55-13.png" width=70%>

## __3. Docsy 테마 기능 추가__
config.toml 파일에 내용을 추가하는 방법으로 진행된다. 

> __config 파일이란__ </br>
일부 프로그램의 매개 변수 및 초기 설정을 구성하는 데 사용되는 파일이다. 사용자 응용 프로그램, 서버 프로세스 및 운영 체제 설정에 사용된다.

### 1) 다양한 언어 지원
Docsy 테마는 다양한 언어를 지원해준다. default 언어를 정하고 그 외의 다양한 언어를 추가할 수 있다. 나는 default 언어는 한국어로 선택하고 영어를 추가했다. 

1. 먼저 root/content 폴더 하위에 언어별 폴더를 만든다. 나의 경우 영어와 한국어를 추가했기 때문에 `en`, `ko` 폴더를 만들었다. content/en, content/ko 구조가 되는 것이다. 앞으로는 이 폴더 안에 컨텐츠를 만들어야 한다. 

2. config.toml에 내용을 추가해준다. 

    나는 default 언어를 한국어로 했다. 그 다음 각 언어별로 타이틀과 directory, time_format을 적어주면 된다.

    아래내용을 복붙하고 타이틀을 바꿔주면 될 것이다. 
```
# Language settings: 한국으롤 default 값으로 했다.
contentDir = "content/ko"
defaultContentLanguage = "ko"
defaultContentLanguageInSubdir = false

# Useful when translating.
enableMissingTranslationPlaceholders = true

# Language configuration
[languages.ko]
title = "한국어 버전 타이틀을 적어주자"
languageName ="Korean"
contentDir = "content/ko"
time_format_default = "2006.01.02"
time_format_blog = "2006.01.02"

# Weight used for sorting.
weight = 1

[languages]
[languages.en]
title = "영어버전 타이틀을 적어주자"
description = "설명을 적어주자"
languageName ="English"
contentDir = "content/en"
time_format_default = "02.01.2006"
time_format_blog = "02.01.2006"
```

### 2) Search 기능
![](https://images.velog.io/images/ceres/post/7ae825d1-928d-4afc-b2c2-238f3a08117c/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202020-06-25%2018-41-29.png)

1. 위에서 언어별로 폴더를 만들었다. 각 언어 폴더 안에 search.md 파일을 만들어 준다. `content/en/search.md` `content/ko/search.md` 

search.md 파일 내용은 아래처럼 작성해 주면 된다.
```
---
title: Search Results
layout: search
---
```

2. search 기능은 2가지 방법이 있다. 나는 local search를 선택했다.

config.toml에 아래 내용을 추가 해주면 된다..
```
# Enable Lunr.js offline search
offlineSearch = true
```

- gcs_engine_id 또는 Algolia DocSearch 내용이 config.toml에 있다면 지우거나 false 값을 줘야한다.

두 가지 방법을 다 입력해주면 충돌이 나기 때문이다.
```
# Google Custom Search Engine ID. Remove or comment out to disable search.
#gcs_engine_id = "011737558837375720776:fsdu1nryfng"

# Enable Algolia DocSearch
algolia_docsearch = false
```

### 3) 페이지 편집 
오른쪽 side bar에 있는 기능이다. `페이지 편집`을 누르면 해당 페이지를 수정할 수 있는 github 페이지가 열린다.
![](https://images.velog.io/images/ceres/post/b55aa2c4-e926-4d88-a8ef-8fcc8c78c3f3/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202020-06-25%2018-44-03.png)

config.toml 에 `github_repo = "레포지토리 주소"` 내용을 넣어주면 되지만 project_repo까지 적었다. 그대로 복붙헤서 레포지토리 주소만 바꾸면 된다.....

```
# Repository configuration (URLs for in-page links to opening issues and suggesting changes)
github_repo = "https://github.com/ceres1105/blog_2"
# An optional link to a related project repo. For example, the sibling repository where your product code lives.
github_project_repo = "https://github.com/ceres1105/blog_2"
```

### 4) footer: 다양한 링크 추가
footer에 메일, 트위터, 스택오버플로우 등 다양한 링크를 추가할 수 있다. 
cofig.toml에 추가하면 된다. 아래 내용에서 사용 링크는 url 주소를 바꾸고, 사용하지 않을 링크는 `#`을 사용해서 주석처리를 하거나 지우면 된다. 

```
[params.links]
# End user relevant links. These will show up on left side of footer and in the community page if you have one.
[[params.links.user]]
	name = "User mailing list"
	url = "https://example.org/mail"
	icon = "fa fa-envelope"
        desc = "Discussion and help from your fellow users"
[[params.links.user]]
	name ="Twitter"
	url = "https://example.org/twitter"
	icon = "fab fa-twitter"
        desc = "Follow us on Twitter to get the latest news!"
[[params.links.user]]
	name = "Stack Overflow"
	url = "https://example.org/stack"
	icon = "fab fa-stack-overflow"
        desc = "Practical questions and curated answers"
# Developer relevant links. These will show up on right side of footer and in the community page if you have one.
[[params.links.developer]]
	name = "GitHub"
	url = "https://github.com/google/docsy"
	icon = "fab fa-github"
        desc = "Development takes place here!"
[[params.links.developer]]
	name = "Slack"
	url = "https://example.org/slack"
	icon = "fab fa-slack"
        desc = "Chat with other project developers"
[[params.links.developer]]
	name = "Developer mailing list"
	url = "https://example.org/mail"
	icon = "fa fa-envelope"
        desc = "Discuss development issues around the project"
```

### 5) Highlight 기능
> Highlight를 넣고 싶은 문장 앞에 ![](https://images.velog.io/images/ceres/post/ff8c78b7-7825-47a3-8020-fbaf5e7e76da/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202020-06-26%2010-44-43.png) 를 적어주고 문장 뒤에 ![](https://images.velog.io/images/ceres/post/e299b2b9-9010-423d-b0b0-cc39f925dc6d/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202020-06-26%2010-46-22.png) 를 적으면 된다.

- 예시) `v` 표시 한 곳만 highlight를 넣고 싶다면 아래처럼 코드를 짜면 된다.

![](https://images.velog.io/images/ceres/post/c7ab9f62-85d9-4e8c-a2aa-b1a87ace8a6e/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202020-06-26%2010-49-06.png)
``` 
[[params.links.user]]
	v name = "User mailing list"
	v url = "https://example.org/mail"
	icon = "fa fa-envelope"
        desc = "Discussion and help from your fellow users"
[[params.links.user]]
	v name ="Twitter"
	v url = "https://example.org/twitter"
	icon = "fab fa-twitter"
        desc = "Follow us on Twitter to get the latest news!"
```
![](https://images.velog.io/images/ceres/post/e299b2b9-9010-423d-b0b0-cc39f925dc6d/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202020-06-26%2010-46-22.png)

- 결과물

![](https://images.velog.io/images/ceres/post/b6563ea2-f75a-4664-a55b-c7989bf477fd/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202020-06-25%2020-10-02.png)

2-3, 7-8번 째 줄에 Highligt 가 된것을 볼 수 있다. 
또 가장 윗 줄이 숫자 1번으로 시작한 것을 볼 수 있다. 


## __4. Docsy 테마 CSS 변경__
Docsy 테마에만 적용되는 점은 아니다. 모든 테마에서 적용된다. 

<span style="color:red"> __가장 중요한 점은 theme 폴더를 직접 건드리지 않아야 한다.__ </span>

theme폴더 내부의 docsy 폴더를 보면 css가 있는 폴더가 있을 것이다. 그 폴더를 root 폴더에 복사 한다.

그렇게 되면 root 폴더에 있는 css 들이 우선순위를 갖게 된다. 바꾸고 싶은 css가 있다면 root 폴더에서 수정하면 된다.

이론은 이러한데 직접해보니 모든 css 파일에 적용되진 않았다. default 색상, 크기, font 정도 수정이 가능했다.

- 자세한 예시 

	scss 폴더는 `root폴더>themes>docsy>assets>scss` 이렇게 들어가면 된다. 나는 assets 폴더 전체를 root 에 복사하였다.  

	여러 파일을 수정해본 결과 수정이 먹히는 파일은  `_variables.scss` 였다. 때문에 이 파일만 옮겨도 될 듯하다. 
`_vairables.scss` 파일에선 default 되어있는 color, font, size 등을 바꿀 수 있다. 

- font 를 바꿔보자

	`_variables.scss` 파일의 font 부분이다.

	![](https://images.velog.io/images/ceres/post/ad8f2cb6-1537-4fe0-835b-a0433cb3606a/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202020-06-25%2020-18-24.png)

	https://fonts.google.com/ 사이트에서 font를 고르고 $google_font_name, $google_font_family 에서 font 이름만 바꿔주면 된다.

	하지만 ` ``` ``` ` 코드 내의 폰트 어떻게 바꿔야 할지 모르겠다. 여기에 한글을 적으면 약간 깨져 보이는 것 같은데 방법을 찾아봐야겠다.

## __5. Docsy 테마 Layout 변경__
footer를 추가하거나 sidebar를 추가하는 등 layout을 수정했다. 

layout도 css와 같다. `root>theme>docsy>layouts`에서 필요한 부분만 `root>layouts`에 복사하면 된다. 

`root>theme>docsy>layouts` 에서 `_default`폴더와 `404.html`,`home.html` 파일을 복사하고, 나머지는 필요한 부분만 복사하면 되는 듯 하다. 나는 sidebar 를 사용할거라 `partials` 폴더에서 section-index.html sidebar_tree.html 을 복사하였다. 