---
title: 'Visual Studio Code(VSCode) Emmet 사용 (JSX Emmet 설정)'
date: 2021-01-10
category: 'Tools'
draft: false
---

![](./images/banner/vscode_banner.jpeg)

## Visual Studio Code(VSCode) Emmet 사용 (JSX Emmet 설정)

근래 들어 React, Vue와 기본적인 HTML, CSS 등 프론트엔드 공부를 많이 진행하면서 여러 강좌를 보다 HTML에서
사용했던 Emmet을 VSCode에서 일반 사용과 React JSX에서도 사용하는 방법을 공유하고자 한다.

## Emmet 이란?

위키에서 HTML,XML,XSL문서 등을 편집할 때 빠른 코딩을 위해 사용하는 플러그인이라고 설명되어있다.
참고 - [위키백과 에밋](<https://ko.wikipedia.org/wiki/%EC%97%90%EB%B0%8B_(%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4)>)

## Emmet 사용

여러 Emmet 문법이 많이 있겠지만 기본적인 사용 몇 개만 설명하고자 한다

### HTML 구조 생성

! 느낌표를 치고 Tab 키를 누르면 아래와 같이 HTML 구조가 자동으로 생성된다.

```html
<!-- ! 후 tab키 -->

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body></body>
</html>
```

<br />

### Element 생성

생성하고 싶은 Element를 문자열로 작성하고 Tab 키를 누르면 아래와 같이 태그가 완성된다.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <!-- div 작성 후 tab -->
    <div></div>
  </body>
</html>
```

Element에 id나 class를 붙여 줄 땐 div#id나 div.class를 입력 후 Enter나 Tab을 눌러준다.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <!-- div#id 후 tab -->
    <div id="id"></div>

    <!-- div.class 후 tab -->
    <div class="class"></div>
  </body>
</html>
```

<br />

### Element 자식 생성

예를 들어 ul 태그 아래 li를 생성할 때 ul>li 후 Tab을 누르면 ul 아래 li요소 1개가 생성되고 ul>li\*5 를 하면 아래와 같이
li 요소 5개가 생성된다.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <!-- ul>li 작성 후 tab-->
    <ul>
      <li></li>
    </ul>

    <!-- ul>li*5 작성 후 tab-->
    <ul>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
    </ul>
  </body>
</html>
```

<br />

### Element 형제 생성

Element 형제 생성은 자식이 > 였다면 형제는 +로 묶어주면 된다. div+p+span 작성 후 Tab을 눌러주면 각 태그가
같은 레벨로 생성된다.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <!-- div+p+span 작성 후 tab-->
    <div></div>
    <p></p>
    <span></span>
  </body>
</html>
```

## VSCode 사용 시 React JSX에서 Emmet 사용

VSCode로 React를 개발할 때 JSX에 Emmet을 사용하기 위해선 settings.json에 아래 소스를 추가해야 한다.

```json
 "emmet.includeLanguages": {
   "javascript": "javascriptreact"
 }
```
