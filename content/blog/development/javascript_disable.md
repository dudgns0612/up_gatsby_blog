---
title: 'disabled 속성 사용하기'
date: 2020-01-14
category: 'JavaScript'
draft: false
---

![](./images/banner/javascript.png)

## disabled 속성 사용하기

어떤 상황에 있어서 버튼을 활성화 시키고 비활성화 시키고싶을 때가 있을 것이다.

- 오류 소스
```js
$('선택자').attr('disabled','true');
$('선택자').attr('disabled','false'); 
```
이방법은 틀린 방법이였다 일단 `disabled` 자체 속성은 비활성화를 하는속성이다.
따라서 `true`든 `false`든 비활성화가 유지된다.
<br /><br />
- 해결방법
```js
$('선택자').removeAttr('disabled'); 
```
위처럼 `disabled` 속성을 지워주는것이다.
이렇게되면 활성화하고 싶을경우 `attribute` 속성을 제거해주면 되고
비활성화해주고 싶을경우 `disabled` 속성을 주면 되겠다.