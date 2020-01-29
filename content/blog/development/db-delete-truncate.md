---
title: 'delete와 truncate차이'
date: 2020-01-13
category: 'Database'
draft: false
---

![](./images/banner/rdbs.png)

## DELETE와 TRUNCATE 차이
- DELETE와 TRUNCATE는 테이블의 내용을 삭제하기 위한 database sql 언어들로 둘은 공통점을 가집니다. 
- 그렇다면 그 차이점은 무엇일까요?
    - **DELETE**는 SELECT,  INSTER, UPDATE와 같이 테이블 데이터를 조작하기 위한 DML(Data Manipulation Language)이고
    - **TRUNCATE**는 CREATE, DROP, ALTER와 같이 스키마를 정의하거나 조작하기 위한 DDL(Data Definition Language)입니다.

<br />
Microsoft Doc을 찾아본 결과 아래와 같이 설명하였습니다.
<br />
<br />

> DELETE 문과 비교하여 TRUNCATE TABLE에는 다음과 같은 이점이 있습니다.
> - 트랜잭션 로그 공간을 덜 사용합니다.
DELETE 문은 행을 한번에 하나씩 제거하고 삭제된 각 행에 대해 트랜잭션 로그에 항목을 기록합니다. 반면 TRUNCATE TABLE은 테이블의 데이터를 저장하는 데 사용되는 데이터 페이지의 할당을 취소하는 방식으로 데이터를 제거하며 페이지 할당 취소만을 트랜잭션 로그에 기록합니다.
> - 일반적으로 적은 수의 잠금이 사용됩니다.
행 잠금을 사용하여 DELETE 문을 실행하면 삭제를 위해 테이블의 각 행이 잠깁니다. TRUNCATE TABLE은 항상 테이블과 페이지를 잠그지만 각 행은 잠그지 않습니다.
> - 빈 페이지는 예외 없이 테이블에 남습니다.
DELETE 문이 실행된 후에도 테이블은 계속 빈 페이지를 포함할 수 있습니다. 예를 들어 힙의 빈 페이지는 최소한 배타적인(LCK_M_X) 테이블 잠금이 있어야만 할당 취소할 수 있으므로 삭제를 위해 테이블 잠금을 사용하지 않는 경우 테이블(힙)에는 빈 페이지가 많이 남게 됩니다. 인덱스의 경우도 삭제 작업 후에 빈 페이지가 남을 수 있지만 이러한 페이지는 백그라운드 정리 프로세스에 의해 신속하게 할당 취소됩니다.
> - TRUNCATE TABLE은 테이블에서 모든 행을 제거하지만 테이블 구조와 테이블의 열, 제약 조건, 인덱스 등은 그대로 남습니다. 테이블 정의 및 테이블의 데이터를 제거하려면 DROP TABLE 문을 사용하십시오.

> 테이블에 ID 열이 포함되어 있으면 해당 열의 카운터는 열에 대한 초기값으로 다시 설정됩니다. 초기값이 정의되어 있지 않으면 기본값인 1이 사용됩니다. ID 카운터를 보존하려면 DELETE를 대신 사용하십시오.

<br />

테스트를 진행해 본 결과 자동 증가 인덱스가 존재하는 테이블에 TRUNCATE를 사용하여 삭제할 경우
인덱스가 자동으로 초기화되지만, DELETE를 사용하여 삭제하는 경우 인덱스가 유지된 상태에서 삭제가 됩니다.  

또한 TRUNCATE는 WHERE절이 없고 ROLLBACK이 불가능하기 때문에 신중하게 사용해야 합니다.

### 참조
> https://docs.microsoft.com/ko-kr/sql/t-sql/statements/truncate-table-transact-sql?view=sql-server-ver15