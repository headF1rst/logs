#papercut 

[[REST API]] 제약조건중 **Uniform interface**의 세부 제약조건 중 하나.

HTTP 응답 메세지만으로 사람이 봤을때 메세지를 온전히 해석할 수 있어야한다. 

메세지가 self-descriptive하기 때문에 클라이언트와 서버의 결합도가 낮아질 수 있으며 둘의 독립적 확장이 가능해진다.
self-descriptive 제약조건은 MediaType으로 `HTML`을 사용하는 웹페이지에선 지키기 쉽지만 `JSON`을 사용하는 API에선 어렵다. 

|-|웹 페이지|HTTP API|
|-|-|-|
|Protocol|HTTP|HTTP|
|커뮤니케이션|사람-기계|기계-기계|
|MediaType|HTML|JSON|

|-|HTML|JSON|
|-|-|-|
|HyperLink|됨 (a 테그)|정의되어있지 않음|
|self-descriptive|됨 (HTML 명세)|불완전|
### HTML

![[스크린샷 2023-11-12 오후 8.37.16.png|400 x 200]]
- 응답 메세지의 Content-Type을 보고 MediaType이 `text/html`임을 확인.
- HTTP를 사용하므로 HTTP 명세에간다. MediaType이 `IANA`에 명세되어있다는걸 확인.
- IANA에서 `text/html` 명세가 `w3.org`에 있다는걸 확인.
- 명세의 테그 해석법을 통해서 메세지를 해석.
### JSON

![[스크린샷 2023-11-12 오후 8.38.08.png|400 x 200]]
- 응답 메세지의 Content-Type을 보고 MediaType이 `JSON`임을 확인.
- HTTP를 사용하므로 HTTP 명세에간다. MediaType이 `IANA`에 명세되어있다는걸 확인.
- - IANA에서 찾은 명세로 이동하여 JSON을 파싱.
- 파싱하더라도 `id`, `title`이 무엇을 의미하는지 모르기 때문에 해석 불가.