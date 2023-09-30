Rest API는 웹의 통신 규약은 HTTP를 이용한다.

### API 는?
API라는 것은 컴퓨터의 기능을 실행시키는 방법(명령이라고도 할 수 있음)을 의미한다. 화면에 "Hello World"를 출력하는 방법은
언어마다 다르다.

Rest API는 내 컴퓨터가 아닌 남의 컴퓨터를 실행시킨다. 예를 들어, 나의 Application이 어떠한 주소로 접속하게 된다면
해당 주소의 데이터를 가져올 수 있게 된다. 물론 가져오는 것뿐만 아니라 수정, 삭제도 가능케 한다.

이렇게 인터넷과 웹을 통해서 나의 컴퓨터를 제어할 때 어떻게 하면 시행착오를 줄이고 더 좋은 API를 만들 수 있는가에 대한
고민의 결과물이 Rest API라고 할 수 있을 것 같다. 참고로 Rest API라는 것은 어떠한 특정 기술을 칭하는 것이 아니다.

### Resource ?
HTTP에서 데이터들을 리소스(Resource)라 부른다. Resource를 Rest API를 통해 표현하자면, 보통 URI를 통해 표현하게 된다.
`https://example.com/testResource`... 이러한 것을 컬렉션이라 부른다. 이 컬렉션을 보통 다수(대량의 데이터)는 
복수형으로 표현하게 된다. 단수형으로는 Element(단건)이라 부른다.

### Method ?
URI의 정보로는 그 어떤 것을 알아낼 수가 없다. URI는 단지 그 정보를 식별하는 이름일 뿐이다.
때문에 이 데이터를 가공해야하는데...그것이 Method다. Rest API는 HTTP 통신 규약을 이용하기 때문에
HTTP가 가지고 있는 Method를 이용한다. (CRUD - POST, GET, PUT/PATCH, DELETE)


보통 Element의 관계에서는 부모가 되는 어떠한 객체(?)를 앞에 두고, 그 다음 해당 Element의 id(다를 수도 있음)를 적으며
그 관계의 자식이 되는 객체를 두는 것을 권고한다. 