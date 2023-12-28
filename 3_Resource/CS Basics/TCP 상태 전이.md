#papercut 

`TCP 상태 전이 다이어그램`
![](https://farm1.staticflickr.com/440/18338404268_f693b065d4_o.png)

1. `close()`를 실행한 클라이언트가 **FIN**을 보내고 **FIN_WAIT1** 상태로 대기한다.
2. 서버는 [[CLOSE_WAIT]] 으로 바꾸고 응답 ACK를 전달한다. 동시에 해당 포트에 연결되어있는 어플리케이션에게 `close()`를 요청한다.
3. ACK를 받은 클라이언트는 상태를 **FIN_WAIT2**로 변경한다.
4. `close()` 요청을 받은 서버 어플리케이션은 종료 프로세스를 진행하고 **FIN**을 클라이언트에 보내 **LAST_ACK** 상태로 바꾼다.
5. **FIN**을 받은 클라이언트는 ACK를 서버에 다시 전송하고 **TIME_WAIT**으로 상태를 바꾼다. **TIME_WAIT**에서 일정 시간이 지나면 **CLOSED** 된다. ACK를 받은 서버 포트를 **CLOSED**로 닫는다.

서버가 먼저 종료하겠다고 FIN을 보낼 수도 있는데, 이런 경우 서버가 **FIN_WAIT1** 상태가 된다.

따라서 클라이언트와 서버가 아닌 `Active Close`, `Passive Close`라는 표현이 정확하다.

**FIN_WAIT2**는 Passive Close로부터 FIN을 받지 못하더라도 일정 시간이 지나면 **TIME_WAIT** 상태가 된다.

`TIME_WAIT`: 소켓이 바로 소멸되지 않고 일정 시간 유지되는 상태. (우분투, CentOS의 경우 기본값이 60초)
### Active Close 측이 연결을 바로 닫지 않고 TIME_WAIT 상태에서 일정시간 대기하는 이유

- 지연 패킷이 발생할 경우를 대비하여 데이터 무결성 문제를 방지하기 위해서.
- 두장치가 연결이 닫혔는지 확인하기 위해서.