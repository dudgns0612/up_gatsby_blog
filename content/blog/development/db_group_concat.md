---
title: 'GROUP_CONCAT을 이용한 컬럼 묶기'
date: 2020-03-20
category: 'Database'
draft: false
---

![](./images/banner/rdbs.png)

#### *Mysql을 예시로 한 포스팅입니다. ####

## GROUP_CONCAT을 이용한 컬럼 합치기
상황에 따라서 GROUP BY 이용 시 특정 컬럼 값을 합쳐야 하는 경우가 있다. 예를 들어 한 사용자가 가질 수 있는 자동차의 대수가 여러 대 일 경우 
하나의 데이터 셋한 줄로 가져오고 싶은 경우 GROUO_CONCAT을 활용하여, 나 원하는 구분자로 문자열로 묶어 반환 할 수 있다.
물론 (JPA)[https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#reference]를 쓰는 사용자들은
SQL문으로 처리하지 않겠지만 나와 같은 사람들을 위하여 블로그 포스팅한다.

## GROUP_BY, GROUP_\_CONCAT 활용
만약에 아래와 같은 데이터셋에 사용자를 묶어 자동차를 ,로 구분 한 문자열로 처리할 경우다.

<table style="border-collapse: collapse; width: 100%; height: 100px;" border="1"><tbody><tr style="height: 20px;"><td style="width: 50%; height: 20px;">USER</td><td style="width: 50%; height: 20px;">CAR</td></tr><tr style="height: 20px;"><td style="width: 50%; height: 20px;">KIM</td><td style="width: 50%; height: 20px;">K5</td></tr><tr style="height: 20px;"><td style="width: 50%; height: 20px;">LEE</td><td style="width: 50%; height: 20px;">TUCAN</td></tr><tr style="height: 20px;"><td style="width: 50%; height: 20px;">KIM</td><td style="width: 50%; height: 20px;">K7</td></tr><tr style="height: 20px;"><td style="width: 50%; height: 20px;">LEE</td><td style="width: 50%; height: 20px;"><span>GRANDEUR</span></td></tr></tbody></table>

```sql
  SELECT USER,
         GROUP_CONCAT(CAR) AS CAR
    FROM USER_CAR
GROUP BY USER 
```
위와 같이 GROUP BY를 통해 묶고 GROUP_CONCAT을 사용하여 CAR의 종류를 ,로 합친 문자열을 반환하게 된다.

<table style="border-collapse: collapse; width: 100%; height: 58px;" border="1"><tbody><tr style="height: 20px;"><td style="width: 50%; height: 20px;">USER</td><td style="width: 50%; height: 20px;">CAR</td></tr><tr style="height: 20px;"><td style="width: 50%; height: 20px;">KIM</td><td style="width: 50%; height: 20px;">K5,K7</td></tr><tr style="height: 18px;"><td style="width: 50%; height: 18px;">LEE</td><td style="width: 50%; height: 18px;"><span style="color: #333333;">TUCAN,<span style="color: #333333;">GRANDEUR</span></span></td></tr></tbody></table>

만약 다른 구분자를 활용하고 싶다면 아래와 같이 SQL문을 변경하여 GROUP_CONCAT에 구분자를 설정하여 준다.
```sql
  SELECT USER,
         GROUP_CONCAT(CAR separator '|') AS CAR
    FROM USER_CAR
GROUP BY USER 
```

<table style="border-collapse: collapse; width: 100%; height: 60px;" border="1"><tbody><tr style="height: 20px;"><td style="width: 50%; height: 20px;"><span style="color: #333333;">USER</span></td><td style="width: 50%; height: 20px;"><span style="color: #333333;">CAR</span></td></tr><tr style="height: 20px;"><td style="width: 50%; height: 20px;"><span style="color: #333333;">KIM</span></td><td style="width: 50%; height: 20px;"><span style="color: #333333;">K5|K7</span></td></tr><tr style="height: 20px;"><td style="width: 50%; height: 20px;"><span style="color: #333333;">LEE</span></td><td style="width: 50%; height: 20px;"><span style="color: #333333;">TUCAN|</span><span style="color: #333333;">GRANDEUR</span></td></tr></tbody></table>

필요에 따라 적적하게 사용한다면 좋은 유익한 Database 함수일 것 같다.

참고 - https://fruitdev.tistory.com/16