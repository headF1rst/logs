#papercut 

`connection`: 프로세스간에 안정적으로 통신할 수 있는 신뢰있는 논리적 통로.

두 process는 통신전에 connection을 열고 ([[3 way handshake]]), 데이터를 주고받은 다음 connection을 닫는다 ([[4 way handshake]])

process간의 통신을 위해서는 각 port를 식별할 수 있는 socket이 필요하고, 한쌍의 socket은 `connection`을 유니크하게 식별한다.

하나의 [[socket]]은 동시에 여러 `connection`들에서 사용될 수 있다.

![[스크린샷 2023-12-29 오전 10.42.02.png]]
(하나의 Host에는 여러 IP address가 있을 수 있다.)