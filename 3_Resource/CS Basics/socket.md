#papercut 

인터넷상의 각 port를 식별하기 위한 고유 주소값.

protocol + ip address + port을 조합한 식별값이다.

TCP/IP 프로토콜 stack에서 application 계층이 system이 지원하는 네트워크를 사용하기 위해서는 커널모드와 같은 권한이 필요하다.

시스템은 어플리케이션이 네트워크 기능을 사용할 수 있도록 프로그래밍 인터페이스를 제공하는데, 이 인터페이스가 `socket`이다.

어플리케이션은 socket을 통해 데이터를 다른 process와 주고받을 수 있다.

TCP/IP 프로토콜 표준에서 정의한 socket과 실제 구현되어 동작하는 socket은 조금 다르다.

TCP/IP 프로토콜에서 TCP냐, UDP냐에 따라 socket 동작 또한 다르다.
### TCP socket 동작 방식

