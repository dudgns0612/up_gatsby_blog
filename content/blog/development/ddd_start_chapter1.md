---
title: '도메인 모델 시작'
date: 2020-01-16
category: 'DDD Start!'
draft: false
---

#### ※DDD Start! 책이 있다는 가정하에 정리한 포스팅입니다.

## 도메인 모델
- 특정 도메인을 개념적으로 표현한 것.
- 도메인 모델은 도메인이 제공하는 기능과 주요 데이터 구성을 포함한다.
- 도메인을 모델링하는데엔 객체, 상태 다이어그램, 수학 공식 등 어떠한 것이든 제약이 없다. 모델링 결과를 모든 이해관계자가 이해하는 것이 중요하다.
- 도메인 모델은 도메인 자체를 이해하기 위한 개념 모델이다. 개발을 위해 구현 모델이 따로 필요하다.

## 도메인 모델 패턴
- 사용자 인터페이스(UI) or 표현(Presentation)
  - 사용자의 요청을 처리 및 결과를 보여줌
  - 사용자는 실제 유저뿐만 아니라 연동하는 외부 시스템이 될 수도 있음
- 응용 (Application)
  - 사용자가 요청한 기능을 실행
  - 업무 로직을 직접 구현하지 않고(!!) 도메인 계층에 구현된 규칙들을 조합해 실행함
- 도메인 (Domain)
  - 시스템이 제공할 도메인의 규칙을 구현
- 인프라스트럭처 (Infrastructure)
  - 데이터베이스, 메시징 시스템 등 외부 시스템과의 연동을 처리

## 도메인 모델 도출
도메인 모델 도출이 무엇인지 생각해보기 전에 도메인 주도 설계의 핵심이 무엇인지 생각해보자.
도메인 주도 설계의 핵심은 모델이 그 가치를 잃지 않고 소프트웨어 개발에 기여하도록 `도메인을 잘 표현한 모델을 만들고`, `함께, 계속해서 그 모델을 바라보는` 것이다.


### 도메인 모델 도출이란?
  - 도메인을 모델링할 때 기본이 되는 작업은 모델을 구성하는 `핵심 구성요소, 규칙, 기능`을 찾는 것이다.
  - 요구사항을 분석하여 메서드, 데이터를 정의하고 찾아내는 것이다.


### 도메인 주도 설계의 핵심을 지키면서 도메인 모델 도출을 하기 위해서는,
  - 모든 사람이 모델 생성에 참가해야 한다. (도메인 전문가, 개발자, 분석가 등)
  - 모델은 항상 여러사람의 검증이 필요한다.
    - 도메인 전문가가 이해할 수 있는가?
    - 다른 개발자가 해당 도메인을 이해할 수 있는가?

이렇게 만들어진 모델은 `문서화`를 통해 공유하여, 누구나 쉽게 접근할 수 있도록 한다.

## 엔티티와 밸류
- 도출한 도메인 모델은 크게 엔티티(Entity)와 밸류(Value)로 구분된다.

## 엔티티(Entity)
- 엔티티는 고유하기 때문에 `식별자`를 갖는다.
- 엔티티 식별자가 같은 두 개의 엔티티는 같은 엔티티라고 판단할 수 있고 엔티티의 속성을 바꾸거나 삭제하기 전까지는 유지된다.
- 이를 바탕으로 equals, hashCode 메소드를 아래와 같이 구현할 수 있다.
```java
public class Entity {
    private String identifier;

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null) return false;
        if (obj.getClass() != Entity.class) return false;
        Entity other = (Entity) obj;
        if (this.identifier == null) return false;
        return this.identifier.equals(other.identifier);
    }

    @Override
    public int hashCode() {
        final int prime = 31;
        int result = 1;
        result = prime * result + ((identifier == null) ? 0 : identifier.hashCode());
        return result;
    }
}
```
## 엔티티의 식별자 생성
- 엔티티 식별자는 특정 규칙 또는 구현 기술에 따라 생성 방식이 달라진다.
- 특정 규칙에 따라 생성
  - 대표적으로 날짜가 있지만 동시시간 생성 이슈에 신경써야한다.
- `8-4-4-4-12` 형태의 16진수 문자열 조합인 UUID 사용 (Java의 경우 `UUID` 클래스 활용)

```java
UUID uuid = UUID.randomUUID();

// 615f2ab9-c374-4b50-9420-2154594af151 과 같은 형태의 문자열 제공
String identifier = uuid.toString();
```

- 값을 직접 입력
  - 회원 ID, 이메일 등
- 일련번호 사용
  - 오라클의 시퀀스 오브젝트, MySQL Auto Increment 컬럼 등

## 밸류(Value)
개념적으로 완전한 하나를 표현할 때 사용한다. 예를들어 아래와 같은 배송정보가 있는 경우를 보자.

```java
public class ShippingInfo {
    // 받는 사람
    private String receiverName;
    private String receiverPhoneNumber;
    // 주소  
    private String shippingAddress1;  
    private String shippingAddress2;  
    private String shippingAddress3;  
}
```
- `receiverName`, `receiverPhoneNumber` 의 경우 서로 다른 정보를 표현하고 있지만, 개념적으로는 **받는 사람**이라는 하나의 정보를 표현하고 있으며 아래와 같이 구현될 수 있다.
- `Receiver`라는 클래스로 밸류 타입을 구현하여 도메인 정보를 보다 더 잘 표현할 수 있다. 

```java
public class Receiver {
    private String name;
    private String phoneNumber;

    public Receiver(String name, String phoneNumber) {
        this.name = name;
        this.phoneNumber = phoneNumber;
    }

    public String getName() {
        return this.name;
    }

    public String getPhoneNumber() {
        return this.phoneNumber;
    }
}
```

- 마찬가지로 `shippingAddress1`, `shippingAddress2`, `shippingAddress3`도 마찬가지로 **주소**라는 하나의 정보를 표현하고 있다.

```java
public class Address {
    private String address1;
    private String address2;
    private String zipcode;

    public Address(String address1, String address2, String zipcode) {
        this.address1 = address1
        this.address2 = address2
        this.zipcode = zipcode
    }
}
```

- 밸류 타입을 활용해 `ShippingInfo`를 다시 구현하면 보다 더 객체지향적이고 명확하게 구현된다.

```java
public class ShippingInfo {
    private Receiver receiver;
    private Address address;

    ...
}
```
- 밸류타입은 꼭 여러 속성을 하나로 묶지 않더라도 의미를 명확하기 위해 사용하기도 한다.
- 아래 예시의 경우 `price`, `quantity`, `amounts` 각각 가격, 수량, 금액을 의미하는데 가격과 금액은 **돈**을 의미하는 값이다.

```java
public class OrderLine {
    private int price;
    private int quantity;
    private int amounts;
    ...
}
```

- 이를 Money라는 밸류를 만들어 사용하면 다음과 같이 표현할 수 있다.

```java
public class Money {
    private int value;

    public Money(int value) {
        this.value = value;        
    }

    public int getValue() {
        return this.value;
    }
}
// int 대신 Money 타입을 사용하여 도메인 속성이 의미하는 바를 보다 명확하게 표현
public class OrderLine {
    private Money price;
    private int quantity;
    private Money amounts;  
}
```
- 또한 이렇게 밸류를 추출하면 해당 밸류가 표현하는 도메인의 규칙 또는 기능을 추가로 구현할 수 있다.
- 이 밸류를 사용하면 구현한 코드에 도메인 모델의 의미를 더 잘 표현할 수 있게된다.

```java
public class Money {
    private int value;

    public Money(int value) {
        this.value = value;        
    }

    public int getValue() {
        return this.value;
    }

    public Money add(Money money) {
        return new Money(this.value + money.value);
    }

    public Money multiply(Money money) {
        return new Money(this.value * money.value);
    }    
}
```
```java
public class OrderLine {
    private Money price;
    private int quantity;
    private Money amounts;  

    public Money calculateAmounts() {
        // 코드가 돈 계산이라는 의미를 갖게됨
        return price.multiply(amounts);
    }
}
```
```java
public class OrderLine {
    private int price;
    private int quantity;
    private int amounts;  

    public int calculateAmounts() {
        // 코드에 돈 계산의 의미는 없고 단지 정수 타입 연산일 뿐
        return price * amounts;
    }
}
```
- 밸류 객체는 보통 불변(Immutable)타입으로 구현하여 변경 기능을 제공하지 않는다.
  - 밸류 객체를 setter 등의 변경 기능을 제공하면 실수나 기타 이유로 밸류 객체의 값이 변할 가능성이 생기고 정합성이 깨지는 상황이 발생한다.
  - 때문에 주로 변경할 값으로 새로운 밸류 객체를 생성하는 방법을 이용한다.
  - 이를 통해 참조 투명성(외부의 변화로 인해 받는 영향도가 없음)과 스레드에 안전 (Thread Safe)하다는 특징을 갖게된다.
- 두 개의 엔티티 객체가 같은지 판별하기 위해 `식별자`를 사용한다면, 밸류 타입은 `타입 내 모든 속성`이 같은지 비교해야한다.

## 엔티티 식별자와 벨류타입
- 엔티티의 식별자는 String인 경우가 많다. 엔티티 식별자를 벨류타입을 통해 도메인 구성요소로의 의미가 잘 드러나도록 하여 가독성을 높힐수 있다.

```java
public class Order {
    private String id;
}
```

```java
// 클래스만 보고 id 필드가 Order 엔티티의 식별자임을 표현
public class Order {
    private OrderNo id;
}
```


## 도메인 모델에 set 메서드 넣지 않기
개발할 때 Vo, Dto 등 클래스를 만들고 습관적으로 getter/setter를 만드는 경우가 많다. 이처럼 도메인 모델에서 의미없이 추가하는 set메소드는 몇가지 문제점을 발생시킨다.
- 도메인 모델의 핵심 개념, 의도를 코드에서 사라지게한다.

```java
public class Order {
    ...
    public void setShippingInfo(ShippingInfo shippingInfo) { ... }
    public void setOrderState(OrderState state) { ... }
}
```
setter를 추가해둔 위의 클래스에서 `setShippingInfo`, `setOrderState`은 단순히 `배송지 정보 설정(set)`, `주문상태 설정(set)`이라는 의미만 가지고 있다. 반면 아래 코드를 보자.

```java
public class Order {
    ...
    public void changeShippingInfo(ShippingInfo shippingInfo) { ... }
    public void completePayment() { ... }
}
```
`changeShippingInfo`, `completePayment`라는 메소드 이름을 통해 `배송지 정보 변경`, `결제 완료`라는 도메인 의미를 갖을 수 있다.

- 또 다른 문제는 도메인 객체를 생성할 때 완전한 상태가 아닐 수도 있다.

```java
    // Order가 생성성되는 시점에서 order는 완벽하지 않음.
    Order order = new Order();

    // set으로 모든 정보를 설정해야 함.
    order.setOrderLine(line);
    order.setShippingInfo(info);

    // 주문자의 정보가 설정되지 않은 상태에서 주문이 완료 됨.
    order.setState(OrderState.PREPARING);
```
도메인 객체가 불완전한 상태로 사용되는 것을 막으려면 생성 시점에 필요한 것을 전달해 주어야 한다.
-> `생성자`를 통해 필요한 데이터를 모두 받아야 한다.
```java
public class Order {
  public Order(Orderer orderer, List<OrderLine> orderLines,
  ShippingInfo shippingInfo, OrderState state) {
      setOrderer(orderer);
      setOrderLines(orderLines);
      setShippingInfo(shippingInfo);
      ... // 그 외 설정
  }

  private setOrderer(Orderer orderer) {
      if (orderer == null) {
          throw new IllegalArgumentException("no orderer");
      }
      this.orderer = orderer;
  }

  private void setOrderLines(List<OrderLine> orderLines) {
      verifyAtLeastOneOnMoreOrderLines(orderLines);
      this.orderLines = orderLines;
      calulateTotalAmounts();
  }

  private void verifyAtLeastOneOnMoreOrderLines(List<OrderLine> orderLines) {
      if (orderLines == null || orderLines.isEmpty()) {
          throw IllegalArgumentException("no orderLines");
      }
  }

  private void calulateTotalAmounts() {
      this.totalAmounts = orderLines.stream().mapToInt(
          x -> x.getAmounts()
      ).sum();
  }
}
```
위 코드의 set 메서드는 접근 범위가 private이기 떄문에 외부에서 데이터를 변경할 목적으로 set 메서드를 사용할 수 없다.
- Immutable한 밸류 타입을 사용하면 자연스럽게 setter를 구현하지 않게된다. 
- 특별한 이유가 없다면, 벨류 타입은 불변으로 구현한다.

## 도메인 용어
도메인 용어를 코드에 반영하지 않으면 코드를 해석해야 하는 부담이 생긴다.
```java
Public OrderState {
  STEP1, STEP2, STEP3... 
}
```
STEP1, STPE2가 각각 무슨 의미인지 알 수 없으며, 코드를 분석하고 이해하는데 시간이 필요하다.
하지만 PAYMENT_WAITING, PREPARING, SHIPPED 와 같이 도메인에서 사용하는 용어를 최대한 반영한다면
쉽게 이해할 수 있고 의미를 변환하는 과정에서 발생하는 버그도 감소시킬 수 있다.
그러므로 도메인 용어에 알맞은 단어를 찾는 시간을 아까워하지말고 적절한 도메인 용어를 사용하기 위해 노력하자.

### 참조
> [최범균,『DDD Start!』, 지앤선(2016)](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=84000742)
