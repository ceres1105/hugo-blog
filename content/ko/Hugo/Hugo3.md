---
title: "Hugo 블로그 만들기(3) : Docsy Theme "
date: 2020-06-18T14:51:48+09:00
type: docs
weight: 4
description: >
 github action 사용해서 hugo blog 만들기 (3)
---

## Docsy 테마
이전 포스트인 Hugo 블로그 만들기 (1),(2)를 기본 theme인 ananke로 만들었다면 배포된 블로그를 볼 수 있을 것이다.
하지만 다른 theme를 사용한 경우라면 page가 생각한대로 나오지 않을 수 도 있고, 
심지어는 페이지 자체가 나오지 않을 수 도 있다!
테마가 다른 경우는 각 테마의 설명을 보고 커스터마이징을 해야한다.

나의 경우 docs 테마를 사용했다.
document page가 있어 보면서 커스터마이징을 진행했다.  [DOCSY SITE](https://www.docsy.dev/docs/)

## Post CSS 설치
`sudo npm install -D --save autoprefixer`
`sudo npm install -D --save postcss-cli`

cnrk
## 컨텐츠 추가하기
- 컨텐츠 루트 디렉토리: `content/`
