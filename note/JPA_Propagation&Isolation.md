JPA에서의 @Transactional, DB에서의 @Transactional이 있다.

이 JPA에서의 Propagation 과 DBMS 의 Isolation을 이해하는 것은 중요하다.

- JPA @Transactional 에서는 주요 옵션이 있다.
  - propagation : Session의 Transaction을 어떻게 이용할지에 대한 옵션이다.
    - REQUIRED, SUPPORTS, MANDATORY, NEVER, NOT_SUPPORTED, REQUIRES_NEW, NESTED 가 있다.
  - Isolation : JPA 상에서 DB 격리 수준 레벨을 지정할 수 있다.
    - DEFAULT, READ_UNCOMMITTED, READ_COMMITTED, REPEATABLE_READ, SERIALIZABLE 가 있다.

### Transaction Propagation 

- propagation 은 Transaction 의 영역을 지정하기 위한 설정이다.<br>
아래는 설정들을 간략하게 나타내었다.

---

- REQUIRED
  - Transaction이 존재할 경우
    - 기존 트랜잭션을 이용한다.
  - 존재하지 않을 경우 
    - 신규 트랜잭션을 생성한다.
  - 기본 설정이다.

```java
@Transactional(propagation = Propagation.REQUIRED)
public void exam(Object obj) {...}
// 또는
@Transactional
public void exam(Object obj) {...}
이 외에도 밑의 예제들도 똑같이 Transactional 에 옵션을 주면 된다.
```

- SUPPORTS
  - Transaction이 존재할 경우
    - 기존 트랜잭션을 이용한다.
  - 존재하지 않을 경우
    - 트랜잭션 없이 수행한다.

- MANDATORY
  - Transaction이 존재할 경우
    - 기존 트랜잭션 이용
  - 존재 하지 않을 경우 
    - Exception 발생
  - 반드시 이전Transaction이 있어야 하는 경우이다.

- NEVER
  - Transaction이 존재할 경우
    - Exception이 발생한다.
  - 존재하지 않을 경우
    - 정상적으로 트랜잭션 없이 수행한다.
  - 트랜잭션 없을 때만 작업이 진행되어야 할 때의 경우

- NOT_SUPPORTED
  - Transaction이 존재할 경우
    - 트랜잭션이 종료될 때까지 대기한 후 트랜잭션이 종료한 후에 실행
  - 존재하지 않을 경우
    - 트랜잭션 없이 로직 수행
  - 기존 트랜잭션에 영향을 주지 않아야 할 때 사용한다.

- REQUIRES_NEW
  - Transaction이 존재할 경우
    - 현재 트랜잭션이 종료될 때까지 대기한 후 새로운 트랜잭션을 생성하고 실행
  - 존재하지 않을 경우
    - 신규 트랜잭션을 생성하고 로직 실행
  - 이전 트랜잭션과 구분하여 새로 트랜잭션으로 실행하고 싶을 때 실행

- NESTED
  - Transaction이 존재할 경우
    - 현재 트랜잭션에 Save Point를 두고 이후 트랜잭션 수행
  - 존재하지 않을 경우
    - REQUIRED 와 동일하게 신규 트랜잭션 생성 후 실행 
  - DBMS 특성에 따라 or 미지원

---

### Isolation Level

동시 Transcation이 수행될 때 다른 Transaction이 동일한 데이터에 대해서 어떻게 보일지에 대한 범위

### 관련용어 

- Dirty Read 
  -  현재 Transaction에서 commit 되지 않은 변경 data를 다른 Transaction이 읽을 수 있음을 의미한다.

- Non-repeatable Read
  - 가장 먼저 data를 읽은 데이터가 다른 Transaction에서 변경을 했고, 이후 다시 이 data를 읽을 때 변경된 data가 읽을 수 있음을 의미한다.<br>
  먼저 변경한 쪽의 데이터를 다시 읽을 수 있음을 의미한다.

- Phantom read 
  - 다른 Transaction이 신규 데이터를 추가하거나, 기존 data를 삭제할 때, 범위 Query를 수행하면 data row가 달라지는 현상

기본으로는, MySQL의 경우 Repeatable READ가 기본으로 되어 있으며, Oracle, SQL Server, Postgres 는 READ COMMIT 이 기본이다.


- READ_UNCOMMITTED Isolation 
  - 가장 낮은 단계의 level 이다.
    - 동시에 동일 데이터에 대해서 Transaction이 수행되는 경우 **결과를 보장하지 못한다.**
    - **계속해서 commit되지 않은 값을 읽을 수 있게 된다.**
    - 따라서 Dirty Read, Non-repeatable Read, Phantom Read 현상 모두 발생한다.


- READ_COMMITTED Isolation (MySQL Default)
  - Dirty Read는 발생하지 않는다.
  - 하나의 트랜잭션에서 **커밋이 수행된 데이터는 다시 읽기를 할 경우 변경된 데이터를 읽는다.**
  - Non-Repeatable Read, Phantom Read 현상이 발생한다.

- REPEATABLE_READ Isolation
  - Dirty Read, Non-repeatable Read 현상 발생하지 않는다.
  - 커밋되지 않는 데이터에 대해서 어떠한 side-effect(부작용)가 없다.
  - 데이터를 다시 읽어도 원래 데이터를 그대로 유지하게 된다.
  - **범위 검색**의 경우 새로 추가된 데이터나, 삭제된 데이터가 보이게 된다.
  - Phantom Read 현상이 발생한다.

- SERIALIZABLE Isolation
  - 가장 높은 단계의 level이다.
    - 동시성 이슈에 관한 side-effect는 없다. 그러나 성능은 매우 떨어진다.
    - 동시에 들어온 Transaction은 모두 serialize 되어 순차적으로 수행되어야 하기 때문

--- 


- propagation 이란?
  - 수행할 때 Trasaction이 어떻게 수행되어야 할지?
  - Transaction을 필요에 따라 구분되어 사용할 수 있게 된다.
- Isolation 이란?
  - DBMS 데이터에 대해서 동시성 이슈가 일어날 때 어떠한 전략을 사용할지 LEVEL을 설정한다.
  - 다른 Transaction 하에서 data를 읽을 때 어떻게 데이터가 보이는지에 대한 설정이다.

따라서 상황에 맞게끔 적절히 사용하는 것이 맞다.

여기에 추가적으로 Transaction 레벨들을 걸게 되었을 때 실제로 어떻게 반영되는지 확인할 수 있으면 좋을 것 같고
동시성에 따라서 DB LOCK 범위가 어떻게 잡히는지도 눈으로 본다면, 이해가 더욱 빠르게 될 수 있을 거 같다.

나중에 된다면, DB LOCK 범위는 꼭.. 알아 보는 것이...