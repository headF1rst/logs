#papercut 

[[POST]], [[GET]], [[PATCH]], [[PUT]], [[DELETE]], [[HEAD]], [[OPTION]]을 지원하며 `https://{serviceRoot}/{Collection}/{id}` 형식의 uri로 구성된 API.

혹은

REST 아키텍처를 준수하는 API.

아키텍처는 제약조건을 의미하며 REST 아키텍처 각각은 세부 아키텍처로 이루어져있다.
대부분의 제약조건은 HTTP를 사용함으로서 만족할 수 있지만, Uniform interface는 그렇지 않다.

REST 제약조건중 **Uniform interface**란 [[self-descriptive]]하고 hyperlink를 통해서 어플리케이션의 상태를 전이할 수 있어야 한다 (**HATEOAS**). 이를 따라야만 클라이언트와 서버의 독립적인 확장이 가능하며 하위 호환성을 유지할 수 있다.

https://www.youtube.com/watch?v=RP_f5dMoHFc