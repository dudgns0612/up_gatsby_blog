---
title: 'proxy 수동 설정(setupProxy)'
date: 2020-02-13
category: 'React'
draft: false
---

![](./images/banner/react.png)

## proxy 수동 설정(setupProxy)

React에서 백엔드 서버로 API 요청 시 호출 할 때 발생 할 수 있는 CORS 관련 오류를 방지하기 위하여 Proxy를
설정하여 준다. 간단하게 설정하는 방법과 수동으로 커스터마이징하는 방법을 정리한다.
테스트는 [VLOEERT BLOG](https://velopert.com/)에서 API 예시를 사용했다.

### package.json를 통한 설정

```json
  "browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  },
  "proxy": "https://jsonplaceholder.typicode.com"
```

### http-proxy-middleware setupProxy.js를 통한 설정

1. 먼저 위에 설정을 수동 설정으로 바꾸기 위해서 http-proxy-middleware설치

```sh
yarn add http-proxy-middleware
```

2. setupProxy.js 파일을 src 밑에 생성 후 아래와 같이 프로젝트 proxy 설정을 커스터마이징 해준다.

```js
// src/setupProxy.js
const proxy = require('http-proxy-middleware')

module.exports = function(app) {
  app.use(
    proxy('/posts', {
      target: 'https://jsonplaceholder.typicode.com', // 비즈니스 서버 URL 설정
      changeOrigin: true,
    })
  )
}
```

proxy 설정을 추가해줌으로 /api로 시작되는 API는 target으로 설정된 서버 URL로 호출하도록 설정된다.
위 설정을 통해서 axios나 fetch로 요청 시에 /posts/1로 호출하게 되면
https://jsonplaceholder.typicode.com/posts/1로 호출하게 된다.

changeOrigin 설정은 http-proxy 모듈의 설명과 같이 대상 서버 구성에 따라 호스트 헤더가 변경되도록
설정하여 준다.

### http-proxy-middleware v1.0 이후 수정사항

http-proxy-middleware가 최근 버전을 업데이트하면서 proxy 설정이 아래와 같이 변경되었으니
해당 모듈 문서와 option들을 [확인](https://www.npmjs.com/package/http-proxy-middleware) 해보고 적용시키도록 하자.

```js
const { createProxyMiddleware } = require('http-proxy-middleware')

module.exports = function(app) {
  app.use(
    createProxyMiddleware('/api', {
      target: 'https://jsonplaceholder.typicode.com',
      changeOrigin: true,
    })
  )
}
```

### pathRewrite 설정

pathRewrite는 서버 API https://jsonplaceholder.typicode.com/posts/1 이라고 가정했을 때
프론트엔드에서 호출 시에는 API에 특정 URL 규칙을 만들어 가독성을 높이고 싶거나 다수의 proxy를
추가해야 할 업무상 프로세스가 존재할 경우 화면에서 API URL path를 말 그대로 대체해주는 역할을 한다.
아래와 같이 설정할 경우 axios나 fetch 설정 시 /api/posts/1로 호출하게 되면 api는 '' 공백으로 대체되어 호출하게 된다.

```js
// src/setupProxy.js
const proxy = require('http-proxy-middleware')

module.exports = function(app) {
  app.use(
    proxy('/api', {
      target: 'https://jsonplaceholder.typicode.com',
      changeOrigin: true,
      pathRewrite: {
        '^/api': '', // URL ^/api -> 공백 변경
      },
    })
  )
}
```
