---
title: 'React ie지원 설정하기'
date: 2020-01-15
category: 'React'
draft: false
---

![](./images/banner/react.png)

## react ie지원 설정하기
create-app-react로 프로젝트로 기본설정을 잡고 시작하였을때 기본적으로 크게 아래와 같은 모듈 설정이 잡힌다.
- Webpack : minify, uglify 등을 포함한 모듈 번들링 도구
- Babel : ES6, React 등의 문법을 ES5 코드로 변환시켜주는 트랜스파일러
- Autoprefixer : 다양한 벤더(브라우저)들에게 적절한 CSS 가 적용될 수 있도록 prefix 를 붙여준다.
- ESLint : 자바스크립트 lint, 코드 컨벤션과 오류 등을 잡아준다.
- Jest : 자바스크립트 테스트 도구
ie11, 9을 지원하기 위해서는 **react-app-polyfill**이 필요하다
babel이 크로스브라우징을 위해 es6 구문들을 es5 구문으로 이해 할 수 있도록 해준다면
polyfill은 es6에서 추가된 **Map**, **Set**, **Promise** 같은 객체를 사용가능하게 바꿔준다.

<br />

먼저 **react-app-polyfill** 모듈을 설치하여 ie에서도 사용이 가능하도록 아래와 같이 설치한다.
```sh
npm i --save react-app-polyfill
```
<br />

설치를 마친 후 src/index.js 코드 맨 윗 줄에 아래 와같이 poltfill을 import 하여준다.
```js
import 'react-app-polyfill/ie11';
import 'react-app-polyfill/stable';
```
<br />

다음으로 package.json에 browserslist 설정에 ie설정을 추가하여준다.
```json
"browserslist": {
    "production": [
      ">0.2%",
      "not dead",
      "not op_mini all"
    ],
    "development": [
      "ie 11",
      "last 1 chrome version",
      "last 1 firefox version",
      "last 1 safari version"
    ]
  }
```

<br />

위와 같은 작업을 마치고도 ie에서 프로젝트가 보이지 않는다면 node_moduies에 있는 .cache를 삭제 후 시도하여본다.

<br />

### 이슈
모든 작업을 마쳤지만 작성자에 경우 에러가 존재하였는데 디버깅을 해보니 아래와 같이 websocket connection 부분이였다.
```js
var connection = new WebSocket(
  url.format({
    protocol: 'ws',
    ...
```

<br />

검색을 통해 **react-scripts 3.3.0**버전에는 http 와 https 모두 WebSockets (WS)를 사용하도록 설정되어 있었고
이로 인한 이슈였다. 감사하게도 다른분의 좋은 자료를 [참고](https://velog.io/@ryu/react-scripts-3.3.0-WebSocket-%EC%9D%B4%EC%8A%88-%ED%95%B4%EA%B2%B0%ED%95%98%EA%B8%B0)하여 **react-scripts 3.2.0** ver으로  수정하여
ie를 지원하도록 설정 할 수 있었다.