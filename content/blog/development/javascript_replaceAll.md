---
title: '정규식을 이용한 replace로 replaceAll 사용'
date: 2020-01-14
category: 'javascript'
draft: false
---

![](./images/banner/javascript.png)

## 정규식을 이용한 replace로 replaceAll 사용

javascript에서 replace는
var text = "foopoo" 문자형 변수가 존재할 때  <br />
`text.replace("o","f")` 로 변환시 맨 처음의 o만 f로 변형되게 된다. <br/>
쉡게 모든 o를 f로 변환하고자 할경우는 정규식을 이용하여 <br/>
replace(/패턴/정규식옵션 , "대체텍스트")형식으로 써주면 된다 <br/>

<br />
옵션에는 g , i , m이있으며 <br />
g - 첫번째 문자만이 아닌 모든 문자를 대체 <br />
i - 대소문자 구분하지 않음 <br />
m - 여러 줄 검색  <br />

<br />

따라서 모든 text를 대체 할 경우 <br/>
`text.replace(/o/g ,"f")` 형식으로 사용하게되면 모든 o문자가 f로 치환된다. <br/>
`text.replace(/ /g, "f")` 모든공백을 f로 치환한다는 뜻이 된다. <br/>