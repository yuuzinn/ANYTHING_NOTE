우리가 평소에 편하게 쓰는 Lombok에 대해서 몇 가지 글을 작성하려고 해요. Java Compile 시점 Lombok에서 지원해주는 어노테이션으로 해당 코드를 추가할 수 있는 강력한 라이브러리예요. 덕분에 코드의 가독성과 간편함을 가져올 수 있어요.


### @Data

이 어노테이션은 @ToString, @EqualsAndHashCode, @Getter, @Setter, @RequiredArgsConstructor 이 다섯 가지의 어노테이션을 한 번에 지원해주는 어노테이션이에요.


#### @RequiredArgsConstructor

언뜻 보기에는 사용하기 좋아 보이지만, 단점들이 존재해요. 먼저 @RequiredArgsConstructor 는 정말 편리하게 생성자를 생성해주지만, 실수하기에 좋아요.

#### @RequiredArgsConstructor
```java
public class Test {
    private String name;
    private String address;
}
Test test = new Test("상훈", "서울");
```

위처럼 간단하게 생성자를 만들었을 경우, 문제가 되지 않아 보여요. 하지만 @AllArgsConstructor, @RequiredArgsConstructor 에는 한 가지 함정이 존재해요. 위의 예제에서 name과 address field 순서를 바꿀 경우 생성자 parameter도 똑같이 바뀐다는 점이에요.
```java
@RequiredArgsConstructor
public class Test {
    private String address;
    private String name;
}

    Test test = new Test("상훈", "서울"); // ??
```
심지어 두 field 타입도 같기 때문에 이러한 경우에는 더더욱 찾아내기가 어려워질 수도 있어요. 때문에 사용할 때에는 꼭 주의해서 사용하는 것이 좋아 보여요. 또다른 방법으로는 Builder를 통해서 생성자를 만들어 내는 것도 하나의 방법이에요.

그리고 사실 RequiredArgsConstructor의 경우 Spring Bean에는 사용한다는 점을 기억해야해요.


#### @EqualsAndHashCode

equals()와 hashCode() 메서드를 만들어 줘요. 다른 블로그를 운영하시는 분들 것들을 참고해보았는데, 적절한 예시는 맞다고 생각들어요. 하지만 equals()와 hashCode() 메서드는 꼭 필요한 상황에서 사용하는 것이 맞다고 생각하기 때문에 굳이 사용하지 않는 곳에 쓸 필요는 없어 보여요.

특히나 equals()와 hashCode()는 둘이 붙어 다니는 개념으로 알고 있는데요. 막상 hashCode를 사용하지 않는 곳에서는 맞출 필요도 없기 때문에 더더욱이 필요 없어 보인다 생각해요. 이거는 지극히 제 주관적 생각이에요.ㅎㅎ


아래는 블로그를 운영하시는 분들 중 똑같은 하나의 예시를 가져 왔어요. 출처를 밝히고 작성하겠습니다.
```java
@EqualsAndHashCode
public static class Order {
    private Long orderId;
    private long orderPrice;
    private long cancelPrice;

    public Order(Long orderId, long orderPrice, long cancelPrice) {
        this.orderId = orderId;
        this.orderPrice = orderPrice;
        this.cancelPrice = cancelPrice;
    }
}

    Order order = new Order(1000L, 19800L, 0L);

    Set<Order> orders = new HashSet<>();
orders.add(order); // Set에 객체 추가

        System.out.println("변경전 : " + orders.contains(order)); // true

        order.setCancelPrice(5000L); // cancelPrice 값 변경
        System.out.println("변경후 : " + orders.contains(order)); // false
        출처 : https://kwonnam.pe.kr/wiki/java/lombok/pitfall
```
#### @Setter

Setter의 경우 필요한 상황이 분명 있겠지만, 그렇지 않을 경우 무분별하게 사용하여 객체의 안정성을 보장받기 힘들어져요. 굳이 필요하지 않는 경우에는 사용하지 않는 것이 객체의 안정성을 보장받을 수 있어요.


그 외에도 여러 주의사항들이 존재하지만, 굳이 사용하지 않는 어노테이션이기도 하고 기본적인 것들이라 넘겨요. 중요한 것은 @Data의 어노테이션은 강력한 기능을 자랑하지만, 이 안에 있는 어노테이션들의 역할들을 잘 파악하고 자신(개발자)이 필요한 어노테이션만 사용해서 올바르게 사용하는 것이 가장 이상적인 개발이라 생각해요.


이렇게 작성하고 보니, 생각보다 깔끔하고 조금 더 객체지향스럽게 코드를 짜는 것은 객체, 메서드를 생성할 때부터 조금 더 고민해보고 작성하는 것이라 생각돼요.