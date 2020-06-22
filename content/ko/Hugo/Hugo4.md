---
title: "Hugo 블로그 만들기(4) : hugo 디렉터리구조"
date: 2020-06-18T14:51:48+09:00
type: docs
weight: 4
draft: false
description: >
 github action 사용해서 hugo blog 만들기 (4)
---

역시 헷갈릴땐 공식문서다! 

왜 때문인지 모르는 부분이 생기면 구글에 검색부터 하고 공식문서는 구글을 다 본 다음에야 온다..

앞으로는 공식문서 부터 맘잡고 찬찬히 들여다보자!

[Hugo 디렉토리 구조](https://gohugo.io/getting-started/directory-structure/#directory-structure-explained)

### archetypes
아키타입. hugo에서 contents를 만들때 `hugo new`명령을 사용하거나 직접 파일을 만든다.
`hugo new` 명령어를 사용하면 파일에 default 되어있는 내용이 있다. archetypes에서는 default 값들을 수정할 수 있다. 

```
//default 내용이다. 
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
draft: true
```

- title의 value인 `{{ replace .Name "-" " " | title }}`는 파일명을 타이틀로 쓴다는 의미이다. 
- draft: true 상태로 되어있는 경우, 해당 페이지는 화면에 나오지 않는다. 아직 미완성 글일 경우 이 상태를 true로 한 후 글이 완성되었을 때 false로 바꾸면 된다. 
만약 draft: true인 상태로 페이지를 화면으로 보고 싶다면 `hugo server -D`를 사용해서 로컬서버에서 보면 된다.

```
//docsy theme에 맞게 내용을 바꾸어 보았다. 
---
title: "{{ replace .Name "-" " " | title }}"
date: {{ .Date }}
type: docs
weight: 
draft: true
description: >
---
```cd ..
### config
구성에 관한 지시들이 JSON, YAML, TOML 파일로 저장되어있다. 환경별로 구성할 수 있다. 

#### Mark up
[기본구성](https://gohugo.io/getting-started/configuration-markup#blackfriday)
공식문서에 나와있는 코드를 거의 다 넣었다..


- Gold Mark
    hugo에서 사용하는 기본 markdown 라이브러리. 빠르다는 장점이 있다.

- Black Friday
     Hugo의 기본 Markdown 렌더링 엔진으로 이제 Goldmark로 대체되었다. 하지만 현재도 사용 가능하다. 

- Highlight
    구문강조

- tableComtents
    목차. Goldmark 랜더러에서만 작동한다. 


### content
사이트의 모든 컨텐츠가 있는 디렉토리이다. 
