# Kafka

## Creat topic 

### Create Client Properties

Run this inside the container.

```shell
cat <<EOF > /tmp/client.properties
security.protocol=SASL_PLAINTEXT
sasl.mechanism=PLAIN
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required \
username="<CLIENT_USERNAME>" \
password="<CLIENT_PASSWORD>";
EOF
```

### Create Topic

Create custom topic, set `topic` name to be used and `retention.ms` time to live in millisecond

```shell
kafka-topics.sh \
--create \
--bootstrap-server localhost:9092 \
--topic <TOPIC> \
--partitions 1 \
--replication-factor 1 \
--config retention.ms=<TTL MS> \
--command-config /tmp/client.properties
```

### List Topic

Get a list of all topics

```shell
kafka-topics.sh \
--bootstrap-server localhost:9092 \
--command-config /tmp/client.properties \
--list
```

### Access Kafka in K8s

```shell
kubectl -n hlaeja-testing exec -it dependency-kafka-controller-0 -- /bin/bash
```
