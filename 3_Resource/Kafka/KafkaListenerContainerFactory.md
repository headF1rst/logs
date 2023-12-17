#papercut 

카프카 컨슈머에 의해서 소비된 메시지를, 비즈니스 로직에 의해 수행될 수 있는 데이터로 가공하기 위한 **전처리**를 담당.

ex) 메시지 필터링, 역직렬화...

```java
@EnableKafka
@Configuration
public class KafkaConsumerConfig {

	@Bean
	public ConsumerFactory<String, String> consumerFactory() {
		Map<String, Object> props = new HashMap<>();

		props.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, bootstrapAddress);
		props.put(ConsumerConfig.GROUP_ID_CONFIG, bootstrapAddress);
		props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
		props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
		return new DefaultKafkaConsumerFactory<>(props);
	}

	@Bean
	public ConcurrentKafkaListenerContainerFactory<String, String> 
		kafkaListenerContainerFactory() {
			ConcurrentKafkaListenerContainerFactory<String, String> factory = 
				new ConcurrentKafkaListenerContainerFactory<>();
			factory.setConsumerFactory(consumerFactory());
			return factory;
	}
}
```

