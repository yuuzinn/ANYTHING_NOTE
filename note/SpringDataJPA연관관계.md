### Spring JPA 연관관계 

#### JPA

Java 진영의 ORM 기술 표준이에요.  ORM은 객체와 RDB 를 매핑하는 기술이에요.


### Entity 매핑

Entity - JPA 를 이용해 DB TABLE 과 매핑할 클래스를 의미해요.

Entity Mapping - Entity 클래스에 DB TABLE 과 컬럼, PK, FK 등을 설정하는 것을 의미해요.


### 연관관계 매핑

- Entity 들은 대부분의 경우 다른 Entity 들과 연관관계를 가져요.

- DB TABLE 은 FK로 Join을 통해 DB TABLE 을 참조해요.

- Entity 는 객체 참조를 이용해 연관된 Entity 를 참조해요.

- 연관관계 매핑 - DB TABLE 의 FK 를 객체의 참조와 매핑하는 것을 의미해요.


### 다중성 (Multiplicity)

- @OneToOne - 일대일 관계 매핑

- @OneToMany - 일대다 관계 매핑

- @ManyToOne - 다대일 관계 매핑

- @ManyToMany - 다대다 관계 매핑


### 방향성

- 단방향 (unidirectional)

- 양방향 (bidirectional)


- 지난 개인/팀 프로젝트에서의 연관관계들은 단방향 or 양방향이었는지 생각해보기

  - 본인은 모두 다 양방향으로 설정하여 개발했었어요.

  - 지금 생각해보니, 의미 없었던 거 같아요.

    - 밑에서 보듯이 객체 그래프 탐색 기능이 추가된 것뿐인데, 그것도 사용하지 않았으며 지금보다 JPA에 대해서 더 모를 때라 무조건 양방향으로만 설정해야 되는 줄로만 알았습니다.


### 연관관계 매핑의 방향성

#### 단방향 vs 양방향

단방향 매핑만으로 연관관계 매핑은 이미 완료되었다고 해도 무방해요.

- 단방향 매핑에 비해 양방향 매핑은 복잡하고, 객체에서 양쪽 방향을 모두 관리해주어야 해요.

- 양방향 매핑은 단방향 매핑에 비해 반대 방향으로의 객체 그래프 탐색 기능이 추가된 것 뿐이에요.


- 사실 그래서 단방향 매핑이면 충분해요. 단방향 매핑을 사용하고 반대 방향으로의 객체 그래프 탐색이 필요할 때 양방향을 사용하는 것이 조금 더 좋은 방향이에요.


아래는 다대일 단방향 연관관계 매핑의 예시예요.

```java

@Entity
public class Member {
    @Id
    @Column(name = "member_id")
    private Long memberId;

    private String name;

    @Column
    private LocalDateTime createDt;
}
```
```java
@Entity
public class MemberDetail {
    @Id
    @Column
    private Long memberDetailId;

    @ManyToOne(cascade = CascadeType.ALL)
    @JoinColumn(name = "member_id")
    private Member member;
    
    private String type;
    
    private String description;
}
```


cascade 속성을 통해 영속성 전이 상태를 지정해줄 수 있어요.


### 영속성 전이

Entity 의 영속성 상태 변화를 연관돤 Entity 에도 함께 적용하는 것이에요.

연관관계의 다중성 지정 시 cascade 속성으로 설정해요.


다음은 일대다 단방향 연관관계 매핑의 예시예요.
```java

@Entity
public class Member {
    @Id
    @Column(name = "memberId")
    private Long memberId;
    
    private String name;
    
    private LocalDateTime createDt;

    @OneToMany(cascade = CascadeType.ALL)
    @JoinColumn(name = "member_id")
    private List<MemberDetail> details;
}
```
```java
@Entity
public class MemberDetail {
    @EmbeddedId
    private Pk pk;

    private String description; 

    @Embeddable
    public static class Pk implements Serializable {
    @Column(name = "member_id")
    private Long memberId;

    private String type;
    }   
}
```

영속성 전이(cascade)를 통한 insert

```java
@Transactional
public void createMemberWithDetails() {
        Member member = new Member("member1", LocalDateTime.now());
        Member savedMember = memberRepository.save(member);

        MemberDetail memberDetail1 = new MemberDetail();
        memberDetail1.setPk(new MemberDetail.Pk(savedMember.getMemberId(), "type1"));
        memberDetail1.setDescription("member1-type1");

        MemberDetail memberDetail2 = new MemberDetail();
        memberDetail2.setPk(new MemberDetail.Pk(savedMember.getMemberId(), "type2"));
        memberDetail2.setDescription("member1-type2");

        member.getDetails().add(memberDetail1);
        member.getDetails().add(memberDetail2);
        }
```
### 양방향 매핑보다 단방향 매핑이 더 좋다는 오해가 있어요. (위의 예시 - 추가적인 update 쿼리 발생)

#### 대부분 단방향이면 충분해요.

- 일대다 단방향 연관관계 매핑에서 영속성 전이를 통한 INSERT

- 일대다 관계의 FK 지정을 위해 추가적인 UPDATE 쿼리문 발생

- 오히려 일대다 양방향 연관관계로 변경하면 추가적인 UPDATE 쿼리가 없어져요.

    - 양방향 연관관계에서는 주인이 누구인지 설정해주어야 해요. 보통 FK를 가지고 있는 쪽이 주인이 돼요. 어노테이션 @MapsId 어노테이션과 @EmbeddedId, @Embeddable 어노테이션들을 처음 봤는데, 아래의 링크에서 간단히 알아보아도 좋을듯 싶어요.

복합키 매핑하기 https://rachel0115.tistory.com/entry/JPA-%EB%B3%B5%ED%95%A9%ED%82%A4-%EB%A7%A4%ED%95%91%ED%95%98%EA%B8%B0-EmbeddedId-MapsId-isNew



- JPA에서 식별자를 둘 이상 사용하려면 별도의 식별자 클래스를 만들어야한다. 이 경우, 식별 관계를 매핑하기 위해 실수했던 부분과 직접 ID값을 할당할 때 발생할 수 있는 문제점에 대해 기록하려고 한다. 🔑 복합키 : 비식별 관계 매핑 - @EmbeddedId 둘 이상의 컬럼으로 구성된 복합 기본 키를 매핑하기 위해서는 별도의 식별자 클래스를 생성해야한다. 다음의 RECRUIT_CATEGORY 테이블은 CATEGORY 테이블의 PK인 category_id와 RECRUITMENT 테이블의 PK인 recruiment_id 를 FK로 ...



### 연관관계 매핑 - 로딩 전략


#### Fetch 전략

- FetchType.EAGER

- FetchType.LAZY

#### 기본 Fetch 전략

- ToMany (@OneToOne, @ManyToOne) : FetchType.EAGER

- ToOne (@OneToMany, @ManyToMany) : FetchType.LAZY

### N + 1 문제

- Entity 에 대해 하나의 쿼리로 N개의 Record 를 가져올 때의 상황

- 연관관계 Entity 를 가져오기 위해 쿼리를 N번 추가적으로 수행하는 문제예요.

이를 해결하기 위해 대표적으로 두 가지 방법이 있어요.

- Fetch Join 활용

- Entity Graph 활용


### Fetch Join 으로 N + 1 문제를 해결할 때에 나올 수 있는 이슈

#### Pagination과 Fetch Join

- Pagination 쿼리에 Fetch Join 을 적용하게 된다면, 실제로 모든 Record를 가져오는 쿼리가 실행돼요.

  - Pagination Limited 수만큼 가져올 것이라 생각하지만, Fetch Join을 통해 연관 Entity 들을 한꺼번에 로딩했을 때 Page 범위 내에 얼만큼의 Record 가 들어가야할지 판단할 수 없어요.

  - Hibernate에서는 limit 조건 없이 전체 Record를 가져오도록 실행한 다음 메모리에서 원하는 Page 만큼 반환하게 돼요.

- 사실 원하는 만큼 반환을 했기에 됐다고 생각을 하지만, 실제 DB 쪽에서는 모든 쿼리가 나가게 되는 상황이 발생하게 돼요. 그렇기에 Pagination과 Fetch Join을 같이 사용할 때에는 분리하여 사용해야만 해요.


### 둘 이상의 컬렉션을 Fetch Join 할 때의 MultipleBagFetchException


#### List 는 기본적으로 Hibernate Bag 타입으로 매핑이 돼요.

- Bag은 Hibernate에서 중복 요소를 허용하는 비순차(unordered)컬렉션이에요.

#### 둘 이상의 컬렉션 (Bag)을 Fetch Join 하는 경우 ?

- 그 결과로 만들어지는 카테시안 곱(Cartesian Product)에서 어느 행이 유효한 중복을 포함하고 있으며 어느 행이 그렇지 않은지 판단할 수 없어 MultiBagFetchException 발생해요.

- 해결할 수 있는 방법으로 List 를 Set 컬렉션으로 변경하여 사용해요(중복방지), @OrderColumn 사용

---

#### 생각해보기

- 일대다 단방향 매핑에서 Insert 할 때에 update 쿼리 왜 나가는지 생각해볼 것

- 개인/팀 프로젝트 연관관계 매핑에서 단방향으로만 지정해줘도 될 것들 알아보고 생각해볼 것

- Pagination과 Fetch Join 관계에서 Fetch Join 말고 Entity Graph로 변경했다면 어땠을지..?

  - 팀 프로젝트에서는 Entity Graph 로 Pagination 했던 것으로 기억하는데 잘 모르겠음. 기억 안 남

- 겪어 봐서 알지만, LAZY 상황일 때 N + 1 문제였던 것에 대해 다시 생각해볼 것

- 카테시안 곱에 대해서 간단하게 알아볼 것