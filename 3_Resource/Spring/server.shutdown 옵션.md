#papercut 

**immediate**

```yml
server:
	shutdown: immediate
```
서버 종료 직전까지 요청받은 값을 아직 다 응답하지 않았더라도 즉시 서버를 종료한다.

때문에 서버A가 서버B에 다수의 요청을하고 서버B가 종료되면 서버A는 보낸 요청에 대한 응답값을 받지 못하기 때문에 네트워크 타임아웃이 발생한다.

**graceful**

```yml
server:
	shutdown: graceful
```
받은 요청에 대한 응답을 모두 마친 뒤 종료.