---
title: 'React Create-React-App(CRA) 환경변수 간단 사용 (dotenv)'
date: 2020-10-16
category: 'React'
draft: false
---

![](./images/banner/react.png)

## React Create-React-App(CRA) 환경변수 간단 사용 (dotenv)

React Create React App(CRA)은 개발자가 Babel이나 webpack 같은 build 도구를 별다른 설치 과정과 설정
없이도 동작할 수있도록 도와준다.
CRA로 React Project를 진행할 경우 dotenv를 포함하게 되는데 dotenv는 project의 환경 별로 프로젝트 구성을
제공할 수 있도록 해준다. 즉, development, test, production.. 등 각 환경에 맞춘 변수들을 제공하여 줄 수 있다.

CRA로 시작하면 dotenv가 package에 포함되어있다.

<br />

## 간단 사용 방법

먼저 dotenv는 .env 파일을 따른다. Project의 src 아래에 아래의 이름으로 파일을 생성한다.

- 개발 환경 변수: .env.development
- 테스트 환경 변수: .env.test
- 운영 환경 변수: .env.production

각각의 이 파일들은 아래와 같은 npmcommand와 연관된다.

- 개발 환경 변수: npm run start
- 테스트 환경 변수: npm run test
- 운영 환경 변수: npm run build

<br />

즉 CRA 생성 시 package.json 내에 script로 선언된 npm command를 보면 start, test, build가 존재하는데
start는 생성 한 .env.development 파일을 읽어온다. test와 build도 각각 개발자가 설정 한 환경변수를 읽게 된다.
따라서 정말 간단하게 환경 별 구성이 필요하다면 유용하게 사용할 수 있다.

각 .env파일에는 REACT_APP으로 시작되게 각각 구성하여 주면 된다.
예를 들어 각 환경 별 바라봐야 하는 CCTV IP 등이 다를 경우 아래와 같이 .env 파일을 생성할 수 있다.

**개발 환경 .env.development file 파일**

```js
REACT_APP_CCTV_IP = 192.168.0.1
```

**운영 환경 .env.production 파일**

```js
REACT_APP_CCTV_IP = 10.2.10.1
```

이렇게 생성하면 각 생성 한 컴포넌트에서 process.env.REACT_APP_CCTV_IP로 불러오면 개발 환경 별로 설정한
값들을 가져오게 된다.
복잡한 환경의 개발 구성에는 다른 방법들과 다른 모듈들이 존재하겠지만 간단하게
환경을 구성하기에 훌륭하다고 생각이 된다.

더 많은 dotenv 설정 및 규칙 등등 의 가이드가 있으니 확인해보고 사용하자.

[**참고 dotenv npm 이동**](https://www.npmjs.com/package/dotenv)
