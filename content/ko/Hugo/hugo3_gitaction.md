---
title: "3. Github Action으로 블로그 자동 배포하기"
date: 2020-06-24T17:59:27+09:00
type: docs
weight: 
draft: false
description: >
    hugo + github action 으로 github.io 페이지 만들기
---

## __Github Action 시작하기__ 
github action은 repository에서 발생하는 이벤트를 처리할 수 있는 인터페이스를 제공한다. repository 별로 가상 서버를 제공하는데, 이 서버를 사용하려면 해당 repository에 대한 접근권한이 있어야 한다. 접근권한을 위해 필요한 것이 github access token이다.

### __1. Github Token__
- 내 github page setting ->  Developer settings -> Personal access tokens 에서 -> Create new token 선택
<img src="https://images.velog.io/images/ceres/post/26aa2332-7925-40c9-bd0a-f3bb8993fe03/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202020-06-19%2016-19-37.png" width=70%>

<br>
<br>

- 토큰을 생성하면 토큰 값이 나오는데 이 값을 꼭 복사해야한다. 
<img src="https://images.velog.io/images/ceres/post/3049a626-ac64-4cfd-b6e6-725e88a0e2ba/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202020-06-19%2016-20-03.png" width=70% style="border: solid lightgray">

<br>
<br>

- 토큰을 사용할 repository -> setting -> seeret -> new secret-> 만든 토큰 이름, 아까 저장한 토큰 값을 적어준다.
<img src="https://images.velog.io/images/ceres/post/f8e7baae-0010-490a-947a-51a749c30283/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7,%202020-06-19%2016-22-54.png" width=70%>

### __2. Github Action__
블로그 배포를 위한 github action을 작성해 보았다. 

```
# action 이름. 원하는대로 정하면 된다. 
name: hugo deploy1


# on: 뒤에오는 event가 발생하면 action이 실행된다. 아래는 master branch에 push 나 pull request가 발생하면 action이 실행되는 코드이다. 보통 그냥 두면 된다. 
on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]

# jobs은 실행될 action을 포함하고 있다.  
jobs:
  
  build:
  	# action은 github에서 제공하는 가상머신에서 실행되는데, runs-on은 가상머신의 환경이다. unbuntu로 되어있는 것을 그대로 두었다. 
    runs-on: ubuntu-latest

    #steps는 명령어 들이다. 
    # uses는 이미 만들어진 action을 사용하는 것, run은 명령어를 실행하는 것이다. 
    steps: 
    
    #1. 가상머신으로 checkout
    - uses: actions/checkout@v2
    
    #2. theme를 submodule로 등록했는데 그것도 checkout
    - name: Checkout submodules
      shell: bash
      run: |
        auth_header="$(git config --local --get http.https://github.com/.extraheader)"
        git submodule sync --recursive
        git -c "http.extraheader=$auth_header" -c protocol.version=2 submodule update --init --force --recursive --depth=1
        # run 다음 내용들은 submodule을 최신으로 udapte한것을 가져오는 내용 + a이다.
        
    #3. npm install 사용하는 theme가 sass/scss를 사용하는 경우 node.js를 설치하고 npm install 과정이 필요하다.
    - name: npm install
      uses: actions/checkout@v2

    - uses: actions/setup-node@v1
      with:
        node-version: '12'
    - run: npm install
    
    #4. Hugo 설치 
    - name: Setup Hugo
      uses: peaceiris/actions-hugo@v2
      with:
        hugo-version: 'latest'
        extended: true
        
    #5. build (public 폴더에 저장 된다.)
    - name: Build Hugo Site
      run: |
        hugo --minify
       # minify는 압축시키는 것을 의미한다. 
    
    #6.Deploy 배포: git token이 필요하다. gh-pages로 publish하는 것 잊지 말자 
    #public 폴더를 github page의 gh-pages 브챈치에 배포한다는 의미이다. 
    - name: Deploy
      uses: peaceiris/actions-gh-pages@v3
      with:
        github_token: ${{ secrets.HUGO_TOKEN }}
        publish_branch: gh-pages
        publish_dir: ./public
```