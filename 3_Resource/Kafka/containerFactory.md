#papercut 

`containerFactory` 속성을 사용하여 [[MessageListenerContainer]]의 구현체 및 구성을 지정할 수 있다.

카프카에서는 기본적으로 `ConcurrentMessageListenerContainer`를 사용하여 멀티스레드에서 메시지를 처리한다.
필요에 따라 다른 컨테이너를 사용하도록 `containerFactory`를 설정할 수 있다.
