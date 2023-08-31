### Value ? Reference

```java
public class test3 {
    private String name;

    public test3(String name) {
        this.name = name;
    }
    
    public void setName(String name) {
        this.name = name;
    }
    
    public String getName() {
        return name;
    }

    public static void main(String[] args) {
        test3 test1 = new test3("상훈");
        foo(test1);
        System.out.println("test1 = " + test1.getName());
    }
    public static void foo(test3 tset3) {
        tset3 = new test3("다인");
//        tset3.setName("다인");
    }
}
```
`출력 : test1 = 상훈`

인스턴스를 만들어 `상훈`이라는 이름을 가졌다. 어떠한 함수를 통해 다시 생성하여 값을 확인하였으나 `상훈`의 값이 나왔다.
확실하게 알아보고자 setter를 활용해서도 해보았으나 값은 똑같다.

왜 그런가? 처음에 인스턴스를 생성(상훈)했고, 그 다음 메서드를 통해 `tset3`이라는 매개변수에 test1이 가진 `주소값을 복사`해서 가진다.
그 다음 새로운 객체를 생성하고 새로 생성된 주소값을 `tset3`이 가지며 `test1`은 그대로 원본 객체를 가르킨다.

Java는 Value언어이다. 데이터 값을 복사해서 함수로 전달하기 때문에 원본 데이터는 데이터가 변경될 가능성이 없다.
하지만 파라미터를 넘길 때마다 메모리 공간을 할당해야 하기 때문에 메모리 공간을 더 잡아 먹는다.


++ 추가로 메모리 구조와 같이 공부할 수 있다.

T 메모리라고 하는데, (스프링 입문책에서 봄) 거기에는 스태틱 영역(클래스), 스택 영역(메서드), 힙 영역(객체(인스턴스))가 있다.
보통 그래서 처음에 main 함수가 실행되면 스태틱 영역에 배치되고 코드를 읽어가며 메모리 영역에 알맞게 쌓아간다. 스택 영역에도 main 스택 프레임을 가진다.
그리고 그 main 스택 프레임 안에 변수들이나 메서드들이 들어가게 된다. if문도 따로 스택 프레임이 존재한다. 그래서 모두 main 스택 프레임 안에서 실행이 된다.

메서드 스택 프레임에서 다른 메서드 스택 프레임의 내부 변수는 서로 접근 불가능하다.

이 부분들은 다시 리마인드할 필요가 있다. 42 페이지부터 다시 확인해 봐야지.