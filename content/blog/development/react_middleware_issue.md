---
title: 'middleware is not a function redux-promise-middleware 이슈'
date: 2020-02-11
category: 'React'
draft: false
---

![](./images/banner/react.png)

## middleware is not a function redux-promise-middleware 이슈
리액트를 다루는 기술을 통하여 미들웨어를 사용하여 비동기 통신하는 부분에서 이슈가 있어 혹시나 개정판이 아닌 리액트를 다루는 기술 도서를 통해서 공부하는 개발자들을 위해 블로그 포스팅을 통하여 정리한다.

### redux-promise-middleware를 통한 redux 비동기 통신 작업 시 middleware is not a function 발생

```js
import { createStore, applyMiddleware } from 'redux';
import modules from './modules';

//import ReduxThunk from 'redux-thunk';
import promiseMiddleware from 'redux-promise-middleware';
import { createLogger } from 'redux-logger';

const logger = createLogger();
const pm = promiseMiddleware({
  promiseTypeSuffixes: [
    'PENDING', 'SUCCESS', 'FAILURE'
  ]
});

const store = createStore(modules, applyMiddleware(logger, pm))

export default store;
```

위에서처럼 책의 예제는 redux-promise-middleware 5.x 버전의 예시라고한다. 따라서 6.x이상부터는 아래와 같이 createPromise로 수정한다.

```js
import { createStore, applyMiddleware } from 'redux';
import modules from './modules';

//import ReduxThunk from 'redux-thunk';
import { createPromise } from 'redux-promise-middleware';
import { createLogger } from 'redux-logger';

const logger = createLogger();
const pm = createPromise({
  promiseTypeSuffixes: [
    'PENDING', 'SUCCESS', 'FAILURE'
  ],
  typeDelimiter: '/'
});

const store = createStore(modules, applyMiddleware(logger, pm))

export default store;
```

침고 - [리액트를 다루는 기술 v1 GitHub issue page](https://github.com/velopert/learning-react/issues/99)