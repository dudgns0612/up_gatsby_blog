---
title: 'var, let, const란? 차이점'
date: 2020-02-08
category: 'JavaScript'
draft: false
---

![](./images/banner/javascript.png)

## var, let, const란? 차이점
var, let, const Javascript에서 변수 선언 방식에 사용 된다는 공통점이 있지만 서로 확연한 차이점이 존재한다.
선언 제약이라던지 변수의 스코프 레벨에 대한 차이점이 존재한다.

<br />

### var
var은 기존에 Javascript에서 사용하던 변수 선언 방식이다. var은 재선언, 재할당등 면에서 제한이 없다.
따라서 여러명의 개발자가 작업을 진행 할 경우 선언한 변수의 중첩등 문제점을 야기할 수있다.
아래와 같이 똑같은 변수를 재선언하여도 문제가 되지않는다.

```js
var name = 'hoons';
console.log(name);  // 출력 : 'hoons'

var name = 'hyuns';
console.log(name); // 출력 : 'hyuns'
```

또한 var은 함수 레벨 스코프를 가진다. 따라서 함수 안에서 변수들은 참조가 가능하다.
아래 예시를 보자.

```js
function scopeCheck() {
    // if에서 선언한 변수 참조가 가능
    if (true) {
        var name = 'hoons';
    }
    console.log(name); // 출력 : 'hoons' 
}
scopeCheck();
```

<br />

### let, const
let과 const는 블록 레벨 스코프를 가진다. 따라서 위에 var와 다르게 if에서 선언한 변수에는
참조가 불가능하다. 따라서 아래 에시에 name은 if의 블록 레벨에서만 유효하게 된다.
아래 예시를 보자.

```js
function scopeCheck() {
    // if에서 선언한 변수 참조가 가능
    if (true) {
        let name = 'hoons';
    }
    console.log(name); // 출력 : Uncaught ReferenceError: name is not defined
}
scopeCheck();
```

<br />

### let, const 차이점
let, const 모두 var과 다르게 재선언이 가능하지 않다. 따라서 블록 스코프 내에서 한번 선언한 변수명은
다시 재선언 할 수 없다. 아래 예시를 보자

```js
function scopeCheck() {
    let name = 'hoons';
    let name = 'hyuns';
    
    console.log(name); // 출력 : Identifier 'name' has already been declared
}
scopeCheck();
```

**그렇다면 let, const차이점은 무엇일까?** <br />
위에 설명처럼 let과 const 모두 재선언이 불가능하고 const는 재할당도 불가능하다.
따라서 Java에서 final 키워드가 붙은것처럼 상수 변수를 정의 할때 사용한다.
재할당을 할 경우 아래와 같이 상수변수에 할당하였다는 오류를 표출한다.
아래 예시를 보자.

```js
    const name = 'hoons';
    // name 재할당
    name = 'hyuns';
      console.log(name); // 출력 : Assignment to constant variable.
}
scopeCheck();
```
