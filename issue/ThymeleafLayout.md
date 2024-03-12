### 타임리프를 사용한 Layout 에서 하나의 문제가 생겼다.

먼저, layout을 사용하려 했던 이유는 중복되는 뷰 페이지를 하나의 layout.html을 통해서 묶어서 사용하기 위함이었다.

하지만 적용하는 데에 문제가 생긴 것이다. layout 자체를 타임리프에서 알아먹질 못했던 것이었다. 알고 보니 타임리프에서 Layout을 사용하기 위한 
의존성 관리가 따로 존재했다. 나는 이를 알지 못했으며, 구글링을 통해 알 수 있었다.

```groovy
   // Thymeleaf layout
    implementation 'nz.net.ultraq.thymeleaf:thymeleaf-layout-dialect'
```

위와 같이 의존성을 추가해주면 된다. 

하지만 그래도 안 됐다. 뭐가 문제였을까? 

```html
<!doctype html>
<html lang="ko">
<head>
    <title>Hello!</title>
</head>
<body>

<th:block layout:fragment="content"></th:block> <!-- 문제의 부분 -->

</body>
</html>
```

딱 layout 부분만 빨간줄 그어져선, 알아먹질 못하는 것이었다. 이래서는 근본적인 문제가 해결되지 않았고, 또다시 구글링을 찾아 헤맸다.
인텔리제이에서 해결해주는 방안으로, html 태그 안에 추가적으로 xmlns 를 입력하라고 되어 있었다.

이게 무슨 말인지 몰라 찾아보게 되었는데, namespace를 안에 정의하시는 분들이 꽤나 있었다.

나도 혹시 몰라 namespace를 아래와 같이 정의해주었더니 해당 문제가 해결되었다.

```html
<html lang="ko" xmlns:layout="http://www.ultraq.net.nz/thymeleaf/layout">
```

layout을 따로 지정해 줘야 되는 건지.. 아직도 잘 모르겠다. 타임리프를 거의 처음 써다 보니까 더 어려운 거 같기도.

