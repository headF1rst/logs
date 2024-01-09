#papercut 

[[ActiveMQ]]에서 `Virtual Topic`을 사용하면 클라이언트는 고유의 토픽을 갖게 된다.

메세지는 해당 `Virtual Topic`을 구독하는 모든 토픽에 전달되고, 각 클라이언트가 오프라인에서 온라인이 될 때 메세지를 소비한다.

덕분에 구독자는 자신의 속도에 맞게 메세지를 소비할 수 있으며 구독자가 오프라인이더라도 메세지가 유실되지 않는다.

VirtualTopic.Order 토픽을 두 서비스가 구독하면 각각 다른 Virtual Topic을 바라보게 된다.

`@JmsListener(destination = "topicName")` 을 메서드에 지정하여 `Virtual Topic`을 구독할 수 있다. 
토픽 에 메세지가 들어오면 해당 메서드가 처리한다.