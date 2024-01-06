#papercut #Network #TODO

[[TCP 상태 전이]] 종료 과정에서 서버는 Active Close 측으로 부터 FIN을 받고 **CLOSE_WAIT** 상태가 되면서 ACK를 전달한다. 동시에 해당 포트에 연결되어있는 어플리케이션에게 `close()`를 요청한다.
### CLOSE_WAIT 종료

이때 어플리케이션에 문제가 발생하여 정상적으로 Active Close에 **FIN**을 보내지 못하게 되면 Active Close의 **FIN_WAIT2** 상태는 시간이 경과함에 따라 **TIME_WAIT**가 되지만 Passive Close측은 **LAST_ACK** 상태로 넘어가지 못하고 **CLOSE_WAIT** 상태에서 머물게 된다.

`CLOSE_WAIT`은 포트를 잡고있는 프로세스의 종료 또는 네트워크 재시작 외에는 제거할 방법이 없다.

어플리케이션이 정상적으로 `close()`를 요청하는것이 가장 좋은 방법.
### CLOSE_WAIT 원인



