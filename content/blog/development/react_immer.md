---
title: 'React Immer 정리'
date: 2020-02-21
category: 'React'
draft: false
---

![](./images/banner/react.png)

## immer 정리

리덕스를 사용하면서 액션 타입, 액션 생성 함수, 리듀서를 수정하면서 state에 새로운 객체 반환을 위해
...을 이용한 전개 연산자와 slice함수를 통하여 객체를 반환하였다. 불변성을 유지하기 위해 다른 포스팅에서
Immutable이라는 라이브러리을 정리했었다. 아래는 Immutable 정리 포스팅이다.

[immutable 정리](https://hoons-up.netlify.com/development/react_immutable/)

<br />

프로젝트를 진행하면서 불변성을 유지하면서 업데이트하는 immer이라는 라이브러리를 발견하여 정리한다.
immer은 immutable과 같이 불변성을 유지를 편리하게 해 준다는 공통점이 있다. 따라서 둘 중
개발자가 편리한 방법을 택하여 사용하면 된다.

### immer 설치
```sh
yarn add immer
```

### immer 예시
```js
import produce from "immer";

const baseState = [
    {
        todo: "Learn typescript",
        done: true
    },
    {
        todo: "Try immer",
        done: false
    }
]

const nextState = produce(baseState, draftState => {
    draftState.push({todo: "Tweet about it"});
    draftState[1].done = true;
});
```

위에서 immer 라이브러리를 import 해주고 baseState 기본 초기 state를 정의했다.
produce에는 2가지 파라미터가 존재하는데 첫 번째 파라미터는 적용 대상인 state 위 소스 코드에서는 baseState를 뜻한다.
두 번째 파라미터는 상태를 어떻게 업데이트할지 정의하는 함수다. 따라서 위에서 draftState에 새로운 object를 push 하였으며
1번째 항목에 done을 true로 바꾼 불변성을 유지한 새로운 상태를 생성해준다.
따라서 아래와 같은 새로운 상태가 반환 되게 된다.

```js
nextState = {
  {
        todo: "Learn typescript",
        done: true
    },
    {
        todo: "Try immer",
        done: true
    },
    {    
        todo: "Tweet about it"
    }
}
```

[immer 문서 참고](https://immerjs.github.io/immer/docs/introduction)
