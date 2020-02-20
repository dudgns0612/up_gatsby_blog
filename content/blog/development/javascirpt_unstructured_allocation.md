---
title: '비구조화 할당(구조 분해 할당) 정리'
date: 2020-02-20
category: 'JavaScript'
draft: false
---

![](./images/banner/javascript.png)

## Javascript 비구조화 할당 정리
구조 분해 할당(비구조화) 구문은 배열이나 객체의 속성을 해체하여 그 값을 개별 변수에 담을 수 있게 하는
JavaScript 표현식아다. 따라서 쉽개 설명하여 [], {}를 형태의 객체를 해제하여 각각 변수에 알맞게 담아준다.

<br />

### 배열 Array
일반적인 방법은 값을 하나씩 배열에서 꺼내와서 담아주는 모습이지만 비구조화 할당은
아래 예시와 같이 배열안에 값을 순서대로 a, b, c에 할당하는 모습을 볼 수있다.

**일반적인 할당 사용**

```js
const arr = [1, 2, 3];

const a = arr[0];
const b = arr[1];
const c = arr[2];

console.log(a); // 1
console.log(b); // 2
console.log(c); // 3
```

**비구조화 할당 사용**

```js
const arr = [1, 2, 3];

const [a, b, c] = arr;

console.log(a); // 1
console.log(b); // 2
console.log(c); // 3
```

**배열 Array 전개구문 사용** <br />
전개구문 ...를 사용하면 나머지 배열 값들을 배열로 생성한다.
```js
const arr = [1, 2, 3];

const [a, ...b] = arr;

console.log(a); // 1
console.log(b); // [2, 3]
```

<br />

### 오브젝트 Object
문법적으로는 배열과 같지만 []가 아닌 오브젝트의 {}를 통해 바구조화 할당을 한다. <br />
***주의해야 할점은 할당 시 오브젝트안에 변수 키값과 비구조화 할당 대상의 변수명이 같아야한다.***

**일반적인 할당 사용**

```js
const obj = {
  one: 1,
  two: 2,
  three: 3
};

// 변수명이 같아야 함
const one = obj.one;
const two = obj.two;
const three = obj.three;

console.log(one); // 1
console.log(two); // 2
console.log(three); // 3
```

**비구조화 할당 사용**

```js
const obj = {
  one: 1,
  two: 2,
  three: 3
};

const {one, two, three} = obj;

console.log(one); // 1
console.log(two); // 2
console.log(three); // 3
```

**오브젝트 Object 전개구문 사용**
```js
const obj = {
  one: 1,
  two: 2,
  three: 3
};

const {one, ...two} = obj;

console.log(one); // 1
console.log(two); // {one: 2, three: 3}
```

[구주 분해 할당 참고 문서](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)