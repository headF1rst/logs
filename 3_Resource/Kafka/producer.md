#papercut 

메세지를 생성하고 특정 [[topic]]에 메시지를 전송할 수 있다.
메세지 전송 성공 여부를 알 수 있으며, 실패시 **재시도가 가능**하다.

의존성 추가로 `producer`와 `consumer`를 사용할 수 있다.
```bash
compile group: 'org.apache.kafka', name: 'kafka-clients', version: '2.3.0'
```

producer는 메세지를 전송할 [[broker]]와 `key`값, 메세지 내용을 지정할 수 있다.
key는 메세지 전송시 [[topic]]의 [[partition]]을 지정할 때 쓰이며 key값은 hash된 값으로 [[partition]]과 매핑된다.

```java
public class Producer {
	public static void main(String[] args) throws IOException {
		// 프로퍼티 객체를 통해 프로듀서 설정을 정의
		Properties configs = new Properties();
		configs.put("bootstrap.servers", "localhost:9092");
		configs.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
		configs.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");

		KafkaProducer<String, String> producer = new KafkaProducer<String, String> (configs);
		ProducerRecord record = new ProducerRecord<String, String>("click_log", "login");
		producer.send(record); // click_log 토픽에 login 메세지가 들어간다.
		producer.close();
	}
}
```

key값을 설정하지 않았다면 `Round-Robin` 방식으로 메세지가 [[partition]]에 분산되어 저장된다. 
key값을 지정하면 key값과 매핑된 [[partition]]에 메세지가 저장된다.

![[스크린샷 2023-11-29 오전 12.00.06.png]]
만약 [[partition]]이 추가되면 key값과의 매핑이 깨져서 데이터가 일관되게 저장되는것을 보장할 수 없다.

key값을 지정하는 경우, 가급적 추후에 [[partition]]을 추가하지 않아야 한다.