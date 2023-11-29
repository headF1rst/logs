#papercut 

일반적인 데이터파이프라인과 달리 kafka는 데이터를 읽더라도 데이터가 사라지지 않는다.

consumer는 [[topic]]의 [[partition]]으로 부터 데이터를 읽으며(polling) 데이터를 읽은 위치를 [[partition]]의 offset으로 커밋한다.

데이터는 여러 consumer로부터 **병렬**로 읽힐수 있다. 단, [[partition]] 개수보다 consumer 수가 많으면 동작하지 않는다.

![[스크린샷 2023-11-29 오전 9.53.47.png]]

consumer group 별로 [[topic]]의 [[partition]]마다 별도의 offset을 가지기 때문에 각기 다른 consumer group에 속한 consumer들은 다른 consumer group의 polling에 영향을 미치지 않는다.

![[스크린샷 2023-11-29 오전 9.55.52.png]]

각 consumer의 offset 정보는 **__consumer_offset** [[topic]]에 저장되어있기 때문에 [[partition]]에 장애가 발생하더라도 수행중이던 offset 지점부터 다시 수행할 수 있다.

```java
public class consumer {
	public static void main(String[] args) {
		// 컨슈머 그룹을 정하고 어느 브로커에서 데이터를 가져올지 선언
		Properties configs = new Properties();
		configs.put("bootstrap.servers", "localhost:9092");
		configs.put("group.id", "click_log_group");
		configs.put("key.deserializer", "org.apache.kafka.common.serializtion.StringDeserializer");
		configs.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");

		KafkaConsumer<String, String> consumer = new KafkaConsumer<String, String>(configs);

		// 어느 토픽에서 데이터를 가져올지 설정
		consumer.subscribe(Arrays.asList("click_log"));

		while(true) {
			ConsumerRecords<String, String> records = consumer.poll(500);
			for (ConsumerRecord<String, String> record: records) {
				println(record.value());
			}
		}
	}
}
```