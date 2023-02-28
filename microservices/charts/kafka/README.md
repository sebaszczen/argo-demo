Kafka current setting can be found here in the pod:\
```cat /opt/bitnami/kafka/config/server.properties```

Default values and documentation can be found here:\
https://kafka.apache.org/documentation/

Choosing the Replication Factor and Partition Count
https://www.conduktor.io/kafka/kafka-topics-choosing-the-replication-factor-and-partitions-count \
https://www.confluent.io/blog/how-choose-number-topics-partitions-kafka-cluster/

# Settings explanation
* Kafka has 3 broker replicas (replicaCount: 3) \
It provides the right balance between performance and fault tolerance
* Each topic replicates to all 3 broker instances (KAFKA_CFG_OFFSETS_TOPIC_REPLICATION_FACTOR, KAFKA_CFG_TRANSACTION_STATE_LOG_REPLICATION_FACTOR, KAFKA_CFG_DEFAULT_REPLICATION_FACTOR)
It provides the right balance between performance and fault tolerance
* KAFKA_CFG_TRANSACTION_STATE_LOG_MIN_ISR:3 With producer.acks=all defined in clients https://gitlab.peoplestrategy.com/peoplestrategy/api-common/-/blob/main/commons-kafka/src/main/resources/application-common-kafka.yml
It ensuer that 2 brokers must acknowledge a write to be considered successful.
One broker can be broken.
* LOG_RETENTION set to 7 days
* 12 partitions per topic
The reasoning behind it is that if have more brokers over time, there will be enough partitions to cover that \
Number of partitions should not be modified as it will break order of messages in partition. 
We use tenantId as message key and it ensures order per partition.
* Each consumer can define concurrency parameter in @KafkaListener annotation. Default value is defined here, by listener.concurrency \
  https://gitlab.peoplestrategy.com/peoplestrategy/api-common/-/blob/main/commons-kafka/src/main/resources/application-common-kafka.yml
