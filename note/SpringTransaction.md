### Transaction 이란
DB 상태를 변환시키는 하나의 논리적 기능을 수행하기 위한 작업의 단위를 뜻한다. 즉, 최소 단위이다.

---

- Transaction은 전파 속성에 의해 결정되어요.
  - Transaction이 진행 중일 때 추가 Transaction 진행을 어떻게 할지? 정하는 것이에요.

- 기존 Transaction이 있고, 새 Transaction이 시작된다면 각각 논리형 외부/내부로 나눠요.
  - Spring에서 Transaction은 이들을 하나의 물리 Transaction으로 묶어요.
    - 이는 내부 Transaction이 외부 Transaction에 참여한다. 라고 표현해요.
    - Spring Transaction
      - 물리 Transaction : 실제 DB에 적용이 되는 Transaction
      - 논리 Transaction : 실제 DB에 적용은 되지 않아요.
    - 모든 논리 Transaction이 Commit 되어야 물리 Transaction이 Commit 되어요.
    - 하나의 논리 Transaction이 롤백된다면, 물리 Transaction은 롤백되어요.
      - 참고로 새 Transaction만이 물리 Transaction을 종료(Commit, roll-back)할 수 있어요.

보통 전파 속성을 Default로 한다면, 하나의 Transaction(예를 들어 Service -> Repository 로직) 안에 또 다른 Transaction이 있을 경우엔 기존 Transaction에 참여해요.

더 자세히 예를 들자면, Service 로직 메서드를 Transaction을 걸어두고, 내부적으로 DB와 직접 연관이 되어 있는 로직(예를 들어 Save) 같은 것들이 있을 경우. 기존 Transaction은 Service에 있는 Transaction이 되는 것이고, 나머지 내부적인 Transaction들은 기존에 참여한다는 뜻이에요.

만약, 기존 Transaciton 안에 참여하는 Transaction이 여러 개라면 어떻게 될까?

예를 들어, 글을 저장하는 Service 로직이 있다고 치죠. 내부적으로는 사진 저장도 있을 수 있고, 로그를 저장하고, 기타 여러 개의 로직들이 저장될 수 있어요.

당연하겠지만 모두 Transaction이 정상적으로 저장이 되어 Commit 된다면 문제가 없지만, 하나의 참여 Transaction이 문제가 발생할 경우 Commit이 되지 않는다.(Default의 경우) 이렇게 된다면, 내부적으로 어떤 오류가 있을 때 사용자는 아무것도 모른 체 글 저장이 왜 안 되는 것인지 의문만 갖게 될 수도 있어요.

그럴 경우 요구사항의 변경이라든지, Refactor 작업이 필요하지 않을까? 글은 저장되더라도 로그 저장은 실패해도 된다든지? Client Interface 에는 아무런 문제가 없이 말이에요.

다른 방법들도 많겠지만, 이럴 때 Transaction 전파 속성을 바꿔 주면 되어요. 기존에는 기존 Transaction에 참여하는 것이었지만, 실패해도 상관없는 로직에는 신규 Transaction을 만들도록(예를 들어 REQUIRES_NEW) 말이에요.
이렇게 설계한다면 기존 Transaction이 어떻든, 서로 다른 Transaction이기 때문에 문제가 되지 않아요.

물론..! 이 방법의 단점도 존재할 테니 각각의 장단점들을 고려하여 선택하는 것이 조금 더 옳은 방법이지 않을까 싶다. 내 생각으로는.. 만약 새 Transaction을 만들어 둔다면, Commit을 두 번 해야 되는 상황이 생겨서 굳이 한 번 해야 할 것을 두 번 하기에 과한 트래픽 같은 것들이 몰릴 경우 문제가 생기지 않을까? 하는 점...??
아니면, Service 로직이 너무 무거워질 수 있으니 따로 분리해서 관리해도 괜찮지 않을까도 싶다. 음... 너무 추상적이지만.. 조금 더 객체지향스럽게...?...ㅎㅎ...

아무튼. 중요한 것은 기존에 @Transacitonal 쓰던 것에서 이러한 전파 속성들이 존재하고, 각각의 상황에 맞게 유동적으로 사용할 수 있다는 것. 또한 논리 Transaction과 물리 Transaction의 차이를 알고, 논리가 하나라도 Roll-Back 된다면 물리도 Roll-Back 된다는 것이에요.