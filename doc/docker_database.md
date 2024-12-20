# Hlæja databases

## InfluxDB

InfluxDB is a high-performance time series database designed to handle large volumes of time-stamped data. It is commonly used for monitoring, analytics, and IoT applications, where data points are associated with timestamps (e.g., sensor readings, system metrics).

### Environment

```text
DOCKER_INFLUXDB_INIT_MODE: setup
DOCKER_INFLUXDB_INIT_USERNAME: influx
DOCKER_INFLUXDB_INIT_PASSWORD: password
DOCKER_INFLUXDB_INIT_ADMIN_TOKEN: influxdbToken==
DOCKER_INFLUXDB_INIT_ORG: hlaeja_ltd
DOCKER_INFLUXDB_INIT_BUCKET: device-data
```

## PostgreSQL

PostgreSQL is a powerful, open-source relational database management system (RDBMS). Known for its reliability and advanced features, it supports SQL for querying and managing data, along with extensive functionality for scalability and extensibility.

### Environment

```text
POSTGRES_USER: postgres
POSTGRES_PASSWORD : password
```

## Apache Cassandra

Apache Cassandra is a distributed NoSQL database designed for handling large amounts of data across many commodity servers with no single point of failure. It is optimized for high availability, scalability, and fault tolerance.

### Environment

```text
CASSANDRA_USER: cassandra
CASSANDRA_PASSWORD: password
```

## Redis

Redis is an in-memory data store that can be used as a database, message broker, or cache layer. It is designed for high performance and low latency, making it suitable for real-time web applications.

### Environment

```text
REDIS_PASSWORD: password
```
