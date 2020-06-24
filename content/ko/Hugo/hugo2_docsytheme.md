---
title: "2. Docsy 테마 사용하기"
date: 2020-06-24T17:27:08+09:00
type: docs
weight: 
draft: false
description: >
    hugo + github action 으로 github.io 페이지 만들기
---
이전 포스트에서 테마를 기본테마인 ananke로 만들었다면 배포된 블로그를 볼 수 있을 것이다.
하지만 다른 theme를 사용한 경우라면 page가 생각한대로 나오지 않을 수 도 있고, 심지어는 페이지 자체가 나오지 않을 수 도 있다. 테마가 다른 경우는 각 테마의 설명을 보고 커스터마이징을 해야한다. 나의 경우 docsy 테마를 사용하였다. 

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

### 2) Docsy 테마 서브모듈로 추가

앞 포스팅과 내용이 같다. hugo site 제작 `hugo new site 폴더명` -> 만든 디렉토리로 이동 `cd 폴더명` -> git 초기화 `git init` -> 서브모듈추가 `git submodule add 깃헙주소 themes/테마이름` -> config.toml 파일에 테마 추가 `echo 'theme = "테마이름"' >> config.toml` -> 로컬로 구축시 필요한 코드 작성`git submodule update --init --recursive`

정리하면 아래와 같다.
```
hugo new site ceres_blog
cd ceres_blog
git init
git submodule add 깃헙주소 themes/docsy
echo 'theme = "docsy"' >> config.toml
git submodule update --init --recursive
```
여기까지 한 후 `hugo server`로 로컬페이지를 보면 화면이 나오지 않는 경우도 있다. 이때는 `npm install`을 해주면 된다. docsy theme가 scss를 사용하기 때문에 npm install을 해야 작동이 된다. 

## __2. Docsy 테마 기능__
다양한 기능은 [DocsyDocument](https://www.docsy.dev/docs/) 에서 확인 가능하다. 참고해서  사용하고 싶은 기능을 사용하면 된다. 이 포스팅에서는 내가 추가한 기능만 정리해 두었다.

### 1) 다양한 언어 지원
Docsy 테마는 다양한 언어를 지원해준다. default 언어를 정하고 그 외의 다양한 언어를 추가할 수 있다. 

## __3. Docsy 테마 CSS 변경__
theme 폴더를 건드리지 않는다. root 폴더에 theme 폴더에 있는 scss를 옮겨주고 root 폴더 내에서 조정하면 된다. 


- 추가할 내용
-  css가 안보이는 오류 : npm install