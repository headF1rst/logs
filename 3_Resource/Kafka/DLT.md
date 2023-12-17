#papercut 

`Dead Letter Topic (DLT)`

컨슈머가 메시지를 브로커로부터 소비하던 도중에 예외가 발생하여 메시지를 소비하지 못했을때, 처리하지 못한 메시지를 `DLT`로 전송한다.

`DLT`는 실패한 메시지에 대한 추적을 가능하게 하고 `DLT`를 사용해서 실패한 메시지에 대해 재시도 혹은 후처리가 가능하다.
### DLT 토픽 네이밍

`DLT 네이밍`: 토픽명 + DLT
ex) OUTSOURCING_REQUEST -> OUTSOURCING_REQUEST.**DLT**

DLT 전략을 위해 사용되는 `DeadLetterPublishingRecoverer`는 메시지를 예외가 발생한 토픽명 + DLT로 네이밍된 토픽에 전송한다.

```java
@Configuration
@RequiredArgsConstructor
public class KafkaConsumerConfiguration {

	private final KafkaProperties kafkaProperties;

	@Value(value = "example.port")
	private String sanhaShopBootstrapAddress;

	@Bean
	public ConcurrentKafkaListenerContainerFactory<String, resultMessage> resultListenerContainerFactory(KafkaTemplate<?, ?> kafkaTemplate) {
	
		JsonDeserializer jsonDeserializer = new JsonDeserializer(OutsourcingResultMessage.class);
		
		return this.concurrentKafkaListenerContainerFactoryWithOutsourcingResultDlt(getConsumerFactoryBysanhaShop(jsonDeserializer), kafkaTemplate);
	}

	private ConcurrentKafkaListenerContainerFactory concurrentKafkaListenerContainerFactoryWithResultDlt(ConsumerFactory sanhaConsumerFactory, KafkaTemplate<?, ?> kafkaTemplate) {
		ConcurrentKafkaListenerContainerFactory<String, ResultMessage> factory = new ConcurrentKafkaListenerContainerFactory<>();
		factory.setConsumerFactory(sanhaConsumerFactory);
		factory.setErrorHandler(new SeekToCurrentErrorHandler(
			new DeadLetterPublishingRecoverer(kafkaTemplate),
			new FixedBackOff(1000L, 2L)));
		return factory;
	}
	
}
```