`KafkaTemplate`: `KafkaProducer` 클래스를 랩핑하고 [[topic]]에 메시지를 전송하는 편리 메서드를 제공하는 인스턴스.

![[스크린샷 2023-10-10 오후 4.16.30.png | 600x250]]
### 생산자 구성

```java
@Configuration
public class KafkaProducerConfig {

	@Bean
	public ProducerFactory<String, String> producerFactory() {
		Map<String, Object> cofigProps = new HashMap<>();
		
		configProps.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, bootstrapAddress);
		configProps.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class);
		configProps.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, StringSerializer.class);
		return new DefaultKafkaProducerFactory<>(configProps);
	}

	@Bean
	public KafkaTemplate<String, String> kafkaTemplate() {
		return new KafkaTemplate<>(producerFactory());
	}
}
```
### kafkaTemplate의 주요 메서드

- **send(topic, key, data)**
`kafkaTemplate.send()`는 `KafkaProducer` 인스턴스의 `send()`를 호출한다.
지정한 [[topic]]으로 메시지를 전송한다.

- **send().get()**
송신된 메시지에 대한 결과를 반환받는다. 
**동기**로 동작하기 때문에 송신 스레드가 blocking 된다.

```java
SendResult<String, OrderInterface> sendResult = orderTemplate.send(
orderTopicName, orderInterface.getOrderCode(), orderInterface).get();
```

- **send().addCallback()**
[[callback]]을 사용한 **비동기**로 동작하기 때문에 후속 메시지가 이전 메시지의 결과를 기다리지 않는다.

```java
private KafkaTemplate<String, String> kafkaTemplate;

public void sendMessage(String message) {

	ListenableFuture<SendResult<String, String>> future = 
		kafkaTemplate.send(topicName, message);

	future.addCallback(new ListenableFutureCallback<SendResult<String, String>>() {

		@Override
		public void onSuccess(SendResult<String, String> result) {
			sout("success");
			// 성공시 후처리 로직 추가
		}

		@Override
		public void onFailure(Throwable ex) {
			sout("fali");
			// 실패시 복구 혹은 로깅 추가
		}
	});
}
```


[Spring을 사용한 Apache Kafka 사용방법](https://memo-the-day.tistory.com/14)