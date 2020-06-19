---
title: "Hugo 블로그 만들기"
date: 2020-06-18T14:51:48+09:00
type: docs
weight: 4
description: >
 github action 사용해서 hugo blog 만들기 (1)
---

## 0. Github에서 repository를 만든 다음 clone을 받는다.

## 1. hugo new site 폴더이름
터미널을 통해 clone 받은 디렉토리로 이동해
  `hugo new site 이름` 명령어를 입력하면 hugo site가 생성된다.
  ```
  //예시 (이름을 blog로 지었다.)
  hugo new site blog
  ``` 
 만들고 나면 나오는 화면이다. congratulations 라고 적혀있다. 

![](https://images.velog.io/images/ceres/post/c6586bc5-ef84-4959-bc4d-2884430e4b2b/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202020-06-19%2015-40-27.png)

## 2. 테마를 받아준다. 
1) 해당 폴더로 가서 git 초기화
`git init`
2) hugo theme 선택하고 github 주소 복사하기

    만약 css 수정할거라면 테마 github 주소 folk 한 다음 folk한 주소 복사하기
3) submodule로 주소 추가하기


    `git submodule add 테마주소 themes/테마이름`

    이렇게 하면 blog 폴더 안에 있는 themes 폴더에 내가 선택한 테마가 clone 되고 submodule에 추가 된다.

```
//예시
// 폴더 들어가기
cd blog

//git 초기화 하기
git init

//submodule추가하기 (나는 docsy라는 테마를 선택했다.)
git submodule add https://github.com/ceres1105/docsy.git themes/docsy
```

## 3. contents 만들기

`hugo new 폴더이름/파일이름.md ` 

주의할 점은 위의 명령어를 config.toml이 있는 최상단 폴더에서 입력해야 한다. 

content 폴더 하위에 폴더가 생성되고 그 하위에 파일이 생성된다. 

만약 `hugo new 파일이름.md` 명령어를 입력하면 content 폴더 하위에 파일이 생성된다.

```
//예시
// 하위 폴더에 있을 경우 상위 폴더인 blog 폴더로 와야한다.
cd ..
// contents 생성. contents폴더 하위에  React폴더를 만들고 Life_Cycle이라는 파일을 만들었다.
//만약 React라는 폴더가 이미 존재했다면 해당 폴더 안에 Life_Cycle 파일이 만들어 졌을 것이다. 
//md 파일은 마크업언어로 작성된 파일이다.
hugo new React/Life_Cycle.md
```

#### Plus
알고 있겠지만, 

파일 내부를 작성하고 싶다면 `code .`를 입력해 VS Code 등을 열어서 내용을 작성해도 되고

`vi 파일명`으로 입력창을 열어서 내용을 작성해도 된다.

vi 입력창은 i를 누른다음 내용을 수정할 수 있다.

수정을 다 한후에는 Esc -> : -> w -> q 를 입력하면 된다. 
(w는 저장, q는 나가기를 의미한다.) 

### 4. 로컬에서 서버 열기

`hugo server -D` 를 입력하고  http://localhost:1313/ 이 주소로 들어가서 내용이 잘 작성되었는지 확인하면 된다. 

- D 는 draft:true인 항목 (=모든 항목)을 로컬서버에서 보여준다는 뜻이다.
- D 가 없다면 draft: false인 항목만 로컬서버에서 보여준다.

### 5. repository로 push
