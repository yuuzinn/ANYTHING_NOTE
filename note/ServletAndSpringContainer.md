Servlet(Container)과 Spring Container에 관해서 알아보자.

Servlet Container는 개발자가 Web Server와 통신하기 위해 Socket을 생성하고, 특정 Port에 Listening, Stream을 생성하는 등의
복잡한 일들을 할 필요가 없게 해준다.

Container는 Servlet의 생성부터 소멸까지의 일련의 과정(생명주기)을 관리한다. Servlet Container는 요청이 들어올 때마다
새로운 Java Thread를 만든다. Tomcat 같은 WAS가 Java 파일을 Compile 해서 Class로 만들고
메모리에 올려서 Servlet 객체를 만든다.

### Servlet 과정

- 사용자가 해당 요청을 보내게 된다면 HTTP Request를 Servlet Container에 보낸다.
- Servlet Container는 HttpServletRequest, Response 두 객체를 생성한다.
- 사용자가 요청한 것을 분석해 어느 Servlet에 대한 요청인지 찾는다.
- Container는 Servlet service() 메서드를 호출, POST/GET 여부에 따라 doGet(), doPost() 둘 중 하나가 호출
- 해당 메서드는 동적 페이지를 생성한 후 HttpServletResponse 객체에 응답을 보낸다.
- 응답이 완료된다면 HttpServletRequest, Response 두 객체를 소멸시킨다.

### Life Cycle

- init : Servlet 초기화
- service : HTTP 요청 유형을 확인하고 그에 맞게 doGet, doPost, doPut 등을 호출하여 처리
- destroy : Servlet 제거

Servlet 객체는 **Singleton으로 관리**되기 때문에 최초 요청 시점에 Servlet 객체를 초기화해서 Servlet Container에 보관하고,
이후에는 같은 Servlet 을 공유해서 사용한다.


##### JSP vs Servlet ??

이 둘의 차이로 기능은 따로 분리된 것은 없고, **역할의 차이**가 있다. 
- JSP는 사용자에게 결과를 보여주는 프리젠테이션 층을 담당하고
- Servlet은 사용자의 요청을 받아 분석하고 비지니스 층과 통신하여 처리 -> 처리한 결과를 다시 사용자에게 응답하는 Controller 단을 담당한다.

