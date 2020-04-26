---
title: 'public 클래스에서는 public 필드가 아닌 접근자 메서드를 사용하라'
date: 2020-04-26
category: 'Effective Java'
draft: false
---

![](./images/banner/effective_java_banner.jpg)

## public 클래스에서는 public 필드가 아닌 접근자 메서드를 사용하라

### 인스턴스 필드들을 모아놓는 클래스 작성
- 아래와 같이 인스스 필드들을 모아놓는 일이외에는 아무 목적도 없는 퇴보한 클래스를 작성할 때 필드는 public이어스는 안된다.
```java
class Point {
    public double x;
    public double y;
}
```
- 이런 클래스는 데이터 필드에 직접 접근할 수 있으니 캡슐화의 이점을 제공하지 못한다. API를 수정하지 않고는 내부 표현을 바꿀 수 없고, 불변식을 보장할 수 없으며, 외부에서 필드에 접근할 때 부수작업을 수행할 수도 없다.
- 철저한 객체 지향 프로그래머는 이런 클래스를 필드를 모두 private으로 바꾸고 public 접근자(getter)를 추가한다.

```java
class Point {
    private double x;
    private double y;

    public Point(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double getX() { return x; }
    public double getY() { return y; }

    public void setX(double x) { this.x = x; }
    public void setY(double y) { this.y = y; }
}
```

- public 클래스에서라면 이 방식이 확실히 맞는 방법이다. 패키지 바깥에서 접근할 수 있는 클래스라면 접근자를 제공함으로써 클래스 내부 표현 방식을 언제든 바꿀 수 있는 유연성을 얻을 수 있다.
- public 클래스가 필드를 공개하면 이를 사용하는 클라이언트가 생겨날 것이므로 내부 표현 방식을 마음대로 바꿀 수 없다. (클라이언트가 필드를 어떤 방식으로 사용하고 있는지 모르기 때문)

### package-private 클래스 혹은 private 중첩 클래스

- pakcage-private 클래스 혹은 private 중첩 클래라면 데이터 필드를 노출한다 해도 하등의 문제가 없다.
- 클라이언트 코드가 이 클래스 내부 표현에 묶이기는 하나, 클라이언트도 어차피 이 클래스를 포함하는 패키지 안에서만 동작하는 코드일 뿐이다.

### 결론

- public 클래스는 절대 가변 필드를 직접 노출해서는 안된다. final로 선언 된 불변 필드라면 노출해도 위험이 덜하지만 완전히 안심할 수는 없다.
- pakcage-private 클래스 혹은 private 중첩 클래스라면 필드를 노출하는 편히 나을 때도 있다.

