---
title: 'react scss 커스터마이징 에러'
date: 2020-01-17
category: 'react'
draft: false
---

![](./images/banner/react.png)

## react scss 커스터마이징 오류

리액트를 다루는 기술이라는 책을 통해 예제를 공부하던 중 아래와 같은 에러가 발생

**'options has an unknown property 'includePaths'. These properties are valid'**

책에는 scss import의 편리함을 위하여 webpack.config를 커스터마이징하여
scss파일을 불러올 때 styles 경로를 잡아주어 절대경로로 불러올 수 있도록 설정하는 방법을
설명해줬다.

참고 - https://velog.io/@velopert/react-component-styling

현재 보고있는 책은 리액트를 다루는 기술에 개정판이 아닌 초판이라 설명이 다른건진 모르겠지만
찾아본 결과 책에는 아래와 같이 options에 추가하도록 나와있지만 현재 react 16.12 ver에선

```js
...
{
  loader: require.resolve('sass-loader'),
  options: {
    includePaths: [paths.styles]
  }
}
```

아래와 같이 options안에 sassOptions로 설정을 해줘야 정상으로 작동하였다.

```js
...
{
  loader: require.resolve('sass-loader'),
  options: {
    sassOptions: {
      includePaths: [paths.styles]
    }
  }
}
```

그외 sass-loader option 참고 - https://github.com/webpack-contrib/sass-loader