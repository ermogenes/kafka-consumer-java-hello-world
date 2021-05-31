# kafka-consumer-java-hello-world
A simple Java Kafka consumer.

## Dependencies (`pom.xml`)

- [kafka-clients](https://mvnrepository.com/artifact/org.apache.kafka/kafka-clients/2.8.0)
- [slf4j-simple](https://mvnrepository.com/artifact/org.slf4j/slf4j-simple/1.7.30)

```xml
        <!-- https://mvnrepository.com/artifact/org.apache.kafka/kafka-clients -->
        <dependency>
            <groupId>org.apache.kafka</groupId>
            <artifactId>kafka-clients</artifactId>
            <version>2.8.0</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.slf4j/slf4j-simple -->
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-simple</artifactId>
            <version>1.7.30</version>
            <!-- <scope>test</scope> -->
        </dependency>
```

## Code

```java
Logger logger = LoggerFactory.getLogger(App.class.getName());

String server = "<your.server.ip.address>:9092";
String topic = "first_topic";
String groupId = "com.example.App";

Properties properties = new Properties();
properties.setProperty(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, server);
properties.setProperty(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
properties.setProperty(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
properties.setProperty(ConsumerConfig.GROUP_ID_CONFIG, groupId);
properties.setProperty(ConsumerConfig.AUTO_OFFSET_RESET_CONFIG, "earliest");

KafkaConsumer<String, String> consumer = new KafkaConsumer<String, String>(properties);
consumer.subscribe(Arrays.asList(topic));

while (true) {
    ConsumerRecords<String, String> records = consumer.poll(Duration.ofMillis(100));
    for (ConsumerRecord<String, String> record: records) {
        logger.info(
            "Message with key: " + record.key() +
            "\nValue: " + record.value() +
            "\nPartition: " + record.partition() +
            "\nOffset: " + record.offset()
        );
    }
}
```
