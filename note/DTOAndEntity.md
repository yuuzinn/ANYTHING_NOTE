예전에 팀 프로젝트를 하던 중에 toEntity에 관해서 이슈가 있었던 적이 있다.

당시 봐 주시던 멘토님께서 리뷰로 `toEntity 대신에 from이나 of를 사용하는 것을 추천한다.`
라고 말씀하셨던 적이 있는데, 솔직히 말해서 거기서 거기 아닌가 ? 싶었다.

그에 대한 답변으로는 아래와 같았다.

>도메인이 분리되어서 말씀하시는 거라면 다른 방법을 생각해 보시는 게 도움이 될 것 같아요.
> 
>원론적으로는 지금 분리한 모듈을 올바르게 분리하였는가가 다시 생각해 볼만한 이슈인 것 같아요.

> 1. *factory method*를 포기한다. 대신 다른 대안을 생각해 본다. (예를 들면 컨버터 클래스라던가, 말씀하시는 생성자라던가, 한곳에서만 사용하고 있다면 private method로 빼는 방법 정도가 생각이 드네요)
> 2. 모듈을 재설계 한다.

>만약 저라면 private method로 만들 것 같기는 합니다.

아직 어떤 패턴(예를 들자면 위의 factory method라던지..)을 보기에는 한참 부족하니, 관련 [링크](https://bcp0109.tistory.com/367)를 걸어두도록 한다.

더 나아가서 DTO에 왜 굳이 toEntity를 두어서 사용해야 하는지 잘 몰랐다.

왜 그렇게 해야되는 것인가?

일단은 Entity와 DTO를 구분하여야 한다. 만약 DTO 없이 entity만 존재한다고 가정한다면
DB에 직접적으로 매핑되어 있는 entity가 어떠한 로직에 의해 변경된다면, 어떤 클래스에 영향을 미칠 것이다.

DTO가 존재한다면 변경사항을 Entity가 아닌 DTO(데이터를 담아두던 곳)에서 처리하고 마지막에 COMMIT 단계에서만 
entity에서 처리되도록 한다면 클래스에 이전보다 덜 영향이 끼칠 것이다.

추가적으로 사용하는 이유들이 몇 가지 더 존재한다.

- entity 내부 구현을 캡슐화할 수 있다.
  - @Getter, @Setter를 통해 충분히 데이터를 변경할 수 있지만, 위에서 작성했듯이 DB와 직접적으로 연관이 되어 있기에
  어떤 클래스(레이어)나 데이터에 큰 영향을 끼칠 우려가 있다. 예를 들면 Controller 단에서 의도치 않게 변경이 됐다던지..
- 자신(개발자)가 필요한 데이터들만 선별하여 데이터를 운반할 수 있다.
  - 최근에 가장 많이 사용했던 팀 프로젝트에서 DTO를 많이 생성해서 사용했었다. 그만큼 Json 파싱으로 필요했던 데이터도 많았고, 그와 동시에
  요구사항으로 필요했던 것이 별도로 있었기 때문이었다. 그래서 필요한 것들만 선별해서 따로 데이터들을 운반할 수 있었다.
- 순환참조(JPA)를 방지할 수 있다.
  - JPA를 사용하면서 Controller 단에서 entity 그대로 응답값으로 보냈던 적이 있었다. 순환참조가 되어
  API 호출이 종료되지 않고, 계속 연관 entity들끼리 서로 참조하는 일이 발생했었는데, **물론 이의 문제는 양방향으로 참조된 entity를 응답값으로 보내줘서 그렇다.**
  하지만 DTO로 응답값을 보내준다면 이런 양방향 참조라고 한들, DTO는 entity가 아니기 때문에 일어날 수 없다.
- Validation 추가
  - Entity 클래스 내부에는 실제로 DB와 연관되어 매핑됐기 때문에 여러 어노테이션으로 작성되어 있다. 예를 들면 
  @Column, @JoinColumn, ... 모델링을 위한 코드들이 추가된다. 그런데 여기에 추가적으로 Validation 의 코드들이 작성되다 보면
  굉장히 가독성을 깨트리고 복잡해질 것이다. <br>
  따라서 DTO 내부적으로 Valdation을 처리하고, Entity 내부에는 DB와 연관된 코드들을 처리함으로써 서로 분리할 수 있게 된다.
