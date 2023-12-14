#papercut 

`MessageListenerContainer`: 카프카에서 메시지를 수신하는 컨테이너.
메세지를 수신하고 처리하는데 필요한 여러 설정들을 관리.

수신한 메세지를 어떻게 가공하고 처리할지에 대한 내용을 담은 팩토리 메서드.
kafkaConsumer 구성객체에 @Bean으로 설정되어 관리된다.

```java
@EnableKafka
@Configuration
public class KafkaConfig {

    @Bean
    public ConsumerFactory<String, String> consumerFactory() {
        Map<String, Object> props = new HashMap<>();
        props.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, "your-bootstrap-servers");
        props.put(ConsumerConfig.GROUP_ID_CONFIG, "your-group-id");
        props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
        props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
        return new DefaultKafkaConsumerFactory<>(props);
    }

    @Bean
    public ConcurrentKafkaListenerContainerFactory<String, String> kafkaListenerContainerFactory() {
        ConcurrentKafkaListenerContainerFactory<String, String> factory =
                new ConcurrentKafkaListenerContainerFactory<>();
        factory.setConsumerFactory(consumerFactory());
        // 여기서 containerFactory의 다양한 설정을 구성할 수 있습니다.
        return factory;
    }
}
```