---
title: 'React create-react-app 절대경로 설정'
date: 2020-02-14
category: 'React'
draft: false
---

![](./images/banner/react.png)

## React create-react-app 절대경로 설정

컴포넌트나 모듈 사용 시 import를 사용해서 가져다 쓰는데 이때 경로설정을 보통은 기본적으로 상대 경로로 한다.
상대 경로 사용 시 디렉토리 구조가 깊어 질수록 ../../../ 을 사용하게 되어 복잡해질 수 밖에 없다.
따라서 아래와 같이 디렉토리 구조에 맞춰 융통성 있게 상대 경로와 절대 경로를 사용 할 수 있도록
절대 경로 설정 방법을 알아보자.

### MacOS 일 경우

package.json 파일에 scripts 부분에 start, build 앞에 NODE_PATH를 추가하여 아래와 같이 수정

```json
  "scripts": {
    "start": "NODE_PATH=src react-scripts start",
    "build": "NODE_PATH=src react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  }
```

### Windows 일 경우
cross-env를 추가해 줘야 제대로 작동한다.

```sh
// yarn 사용일 때
yarn add cross-env

// npm 사용일 때
npm install cross-env
```

scripts 부분의 start, build 앞에 cross-env 와 NODE_PATH 추가

```json
  "scripts": {
    "start": "cross-env NODE_PATH=src react-scripts start",
    "build": "cross-env NODE_PATH=src react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  }
```

**! create-react-app 기준 설정으로 NODE_PATH로 설정 된 경로가 자동으로 resolve되어 별개의 webpack 설정이 필요없음**