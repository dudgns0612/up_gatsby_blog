---
title: 'React Immutable 정리'
date: 2020-01-22
category: 'React'
draft: false
---

![](./images/banner/react.png)

## Immutable 정리
리엑트 공부 중 리덕스를 사용하면서 액션 타입, 액션 생성 함수, 리듀서를 수정하면서 새로운 객체 반환을 위해
...을 이용한 전개연산자와 slice함수로 새로운 수정된 새로운 객체를 반환 하였다.
책에서 이러한 불편함을 편리하게 하기 위해서 페이스북 팀이 만든 라이브러리인 Immutable.js 정리가 되어있어
정리 포스팅을 작성한다.

<br />

### Immutable 설치
```js
npm run immutable
```

<br />

### Immutable 필요성
이미 알고 있는 사실이겠지만, 아래와 같이 객체 불변성을 설명한다.
```js
let obj1 = {a: 1};
let obj2 = {a: 1};

obj1 === obj2  
// false

let obj3 = obj2;  
obj3 === obj2;  
// true 같은 객체를 가르킴

// 따라서 변경 시 같이 변경 됨  
obj2.a =2;  
obj3  
// {a: 2}
```

리엑트의 컴포넌트는 state 또는 상위 컴포넌트에서 전달받은 props 값이 변할 때 리렌더링 된다.
만약 객체나 배열을 직접 위에 객체를 예를들어 obj1.a = 3 으로 내부 값을 직접 수정한다면 
레퍼런스가 가리키는 곳이 같기 때문에 똑같은 값으로 인식한다.
따라서 전개연산자나 assgin 함수를 이용하여 새로운 객체나 배열을 만들었다.
하지만 아래와 같이 복잡한 객체 구조나 배열일 경우에 그 작업이 간단하지 않을것이다.
```js
const obj1 = {
    a: 1,
      b: 2,
      c: {
        d: 3,
          e: {
            f: 4
        }
    }
}

// f값을 5로 업데이트 객체 반환
const obj2 = {
    ...obj1,
      c: {
        ...obj1.c,
        e: {
            f: 5    
        }
    }
}
```

이러한 작업을 간소화하기 위해서 Immutable.js 라이브러리를 사용한다.
위 코드를 Immutable 사용하여 아래와 같이 변경 할 수있다.

```js
import { Map } from 'immutable';

const obj1 = Map({
    a: 1,
      b: 2,
      c : Map({
        d: 3,
          e: Map({
            f: 4
        })
    })
})

// f를 5로 업데이트 새로운 객체 반환
const obj2 = obj1.setIn(['c', 'e', 'f'], 5);
```
위에 소스보다 훨신 간결하고 보기 쉬워졌다는 걸 느낄 수 있다.

<br />

### Map
Immutable의 Map은 객체 대신 사용하는 데이터구조로 javascript에서 사용되는 Map과는 다르다.
```js
import { Map, fromJS } from 'immutable';

const obj1 = Map({
    a: 1,
      b: 2,
      c : Map({
        d: 3,
          e: Map({
            f: 4
        })
    })
})

const obj1 = fromJS({
      a: 1,
      b: 2,
      c: {
        d: 3,
          e: {
            f: 4
        }
    }
})
```

위처럼 일일히 Map으로 감싸주기 힘들 때는 fromJS를 사용하여 객체를 만든다.
fromJS를 사용하면 내부 객체를 Map으로 만들어 준다.
Map으로 감싸주지 않으면 set,get과 같은 Immutable Map의 함수를 활용할 수 없어 Map이나 fromJS를 사용하여야한다.

Map을 console로 찍어보면 Object형태로 나열되어 보일탠데 객체를 작성한 일반형태로 확인하고 싶다면
아래와 같이 변형하여 확인한다.
```js
const obj1Json = obj1.toJS();
// {a:1, b:2, c: {d: 3, e: {f: 4}}}
```

아래는 Map의 값의 조회, 수정, 동시 수정에 대해 간단히 설명한다.
```js
import { Map } from 'immutable';

const obj1 = Map({
    a: 1,
      b: 2,
      c : Map({
        d: 3,
          e: Map({
            f: 4
        })
    })
})

// a값을 불러오기
obj1.get('a') //1

// 더 안에 값 불러오기
obj.getIn(['c', 'd']) //3

// a값 수정하여 새 객체 생성
const newObj1 = obj.set('a', 2);

// 더안에 d값 수정하여 새 객체 생성
const newObj1 = obj.setIn(['c', 'd'], 5);

// 여러 값 동시 수정
// set 활용 동시 수정
// merge나 mergeIn으로 사용 가능하지만 속도 면에서 set, setIn이 좋다.
const newObj1 = obj1.set('a', 1).
                  .set('b', 2);

// merge 활용 동시 수정
const newObj1 = obj1.merge({
    a: 1,
    b: 2
});
```

<br />

### List
LIst는 기존 Immutable에서 배열 대신에 사용한다. 따라서 기존 javascript 배열과 같이
map, filter, sort, push, pop 등의 함수를 내장하고 있다.
**Immutable의 Map, List는 항상 새로운 객체를 반환한다는 사실을 기억하자**
```js
import { Map ,LIst, fromJS } from 'immutable';

// List 생성
const list = LIst([0, 1, 2, 3, 4]);

// Map을 이용한 객체 LIst 생성
const list = LIst([
    Map({ a: 1 }),
    Map({ b: 21})
]);

// fromJS를 이용한 객체 LIst 생성
const list = fromJS([
    {a: 1},
      {b: 1}
]);

// 0번째 값 or 객체 불러오기
list.get(0)

// 0번째 객체 a값 불러오기
liet.getIn([0, 'a']);

// 0번째 객체 수정하기
const newList = list.set(0, Map({a: 2}));

// 0번째 객체 내부 값 a를 3으로 수정하기
const newList = list.setIn([0, 'a'], 3);

// 아이템 맨 뒤 추가하기
const addList = list.push(Map({c: 1}));

// 아이템 맨 앞 추가하기
const addList = list.unshift(Map({c: 1}));

// n번째 아이템 제거하기 (0번째 아이템 제거)
const deleteList = list.delete(0);

// 마지막 아이템 제거
const deleteList = list.pop();

// list 사이즈 가져오기
console.log(list.size);
```

<br />

[Immutable GitHub 참고](https://github.com/immutable-js/immutable-js)

[참고 문헌 - 리액트를 다루는 기술](http://www.yes24.com/Product/Goods/62597469)