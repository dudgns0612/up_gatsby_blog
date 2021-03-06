---
title: 'JavaScript 세미콜론 정리'
date: 2020-07-02
category: 'JavaScript'
draft: false
---

![](./images/banner/javascript.png)

## JavaScript 세미콜론 여부

코딩 테스트 관련 문제를 찾아보던 도중 세미콜론에 대한 긍정적, 부정적인 부분의 글을 읽었다. 
JavaScript 뿐만 아니라 C, JAVA도 문장의 끝에 세미콜론을 붙인다 JAVA에서는 세미콜론을 코드의 끝에
명시해주지 않을경우 컴파일 에러가 발생한다. 

## 세미콜론에 대한 막연한 생각

지금까지 'JavaScript에선 세미콜론을 붙이지 않는다고 JAVA에서처럼 에러가 나진 않는다.' 정도만 이해하였고 
현재 Vue.js로 스티디 팀원들과 간단한 토이 프로젝트를 진행 중이다. 지금까지 세미콜론에 대한 생각이 뚜렷하게 없었고, 
템플릿의 소스에 세미콜론이 존재하지 않았기 때문에 기존과 다르게 세미콜론을 사용하지 않고 작업중이다. 
그에 따라 느낀 생각은 지금까지는 세미콜론을 붙여 작업을 진행했는데 세미콜론 없이 작업하다 보니 
소스의 가독성이 떨어지는 느낌을 받았다. 물론 코드가 기존과 다르게 느껴져 그랬을수도 있지만 
뚜렷한 나의 생각이 없었기에 정리가 필요하다고 느껴 다른 블로그를 찾아보고 정리해보려한다. 

## 세미콜론을 사용하지 않았을 경우 잠재적 문제점

자바스크립트는 문장과 끝을 세미콜론으로 구분한다는 점은 이미 모두가 알고 있는 사살이다. 하지만 이것이 
실행에 문제가 되지는 않는다. 그 이유는 인터프리터 과정에서 자동으로 문장의 끝에 세미콜론을 붙여주기 때문이다.
다른 블로그를 찾아보던 중 세미콜론을 사용하지 않았을 경우 잠재적 문제점에 좋은 예를 찾았다.

### 문제점
아래는 세미콜론을 사용하지 않은 함수의 예시다 보통 함수안에 정의된 Object가 표출될 것이라 생각할 것이다.

```js
const test = () => {
 return 
  {
   isSemi : true
  }
}
console.log(test())
```

하지만 결과물은 **undefined**를 표출한다 그 이유는 인터프리터가 자동으로 아래와 같이 return에 세미콜론을 붙이기 때문이다.

```js
const test = () => {
 return;
  {
    isSemi : true
  }
}
console.log(test())
```

물론 이 점은 말 그대로 잠재적인 이슈일 뿐 아래와 같이 코딩한다면 세미콜론 여부와 상관없이 작동한다.

```js
const test = () => {
  return {
    isSemi : true
  }
}
console.log(test())
```

또 다른 예시를 보자 아래 결과물은 정상적으로 작동할까? 결과는 **Cannot access 'b' before initialization** 에러가 발생한다.

```js
const a = 1
const b = 2
(a+b).toString()
```

위 코드는 아래와 같이 변경되어 b를 찾을 수 없게 된다.

```js
const a = 1;
const b = 2(a+b).toString();
```

## 세미콜론을 사용하지 않는다는 주장

여러 많은 개발자들은 세미콜론의 사용을 고집하는 것에 대하여 반대한다. 간단하게 설명하면
세미콜론은 코딩 스타일에 따른 문제점이고 세미콜론을 붙이지 않고 코드 자체의 품질과 이해를 하는 것이
중요하다는 점과 세미콜론을 강요하는 규칙이 생산성을 헤친다는 것이다.

또 무작정 문장의 끝에 세미콜론을 붙이는 게 아니라 붙이지 말하야할 위치가 있기 때문이다.
예를 들어 아래와 같이 함수의 끝에는 세미콜론을 붙이는 건 좋지 않다.

```js
const test = () => {
  const up = 'hoons';
}; <- 좋지않다.
```

## 정리
세미콜론의 사용은 역시나 Javsscript에서 강요되지 않으며 개발자의 스타일대로 작성하는 게 맞는 부분인 거 같다.
하지만 그에 따라 발생할 수 있는 이슈들에 대해 신경 쓰고 하나로 통일하는 게 좋다고 생각한다.
세미콜론을 붙이지 않았을 경우 발생할 수 있는 부분들은 '[ {' 등으로 시작하는 문장이 없도록 하거나
정확한 코드의 이해를 하고 작성한다면 어느 쪽이든 문제가 없다고 생각한다.

## 참고
[자바스크립트, 세미콜론을 써야 하나 말아야 하나](https://bakyeono.net/post/2018-01-19-javascript-use-semicolon-or-not.html) <br />
[JavaScript에서 명시 적 세미콜론이 중요한 이유](https://www.freecodecamp.org/news/codebyte-why-are-explicit-semicolons-important-in-javascript-49550bea0b82/)