# Hlæja Development

Services and networks, to shape and to steer, Containers in harmony, their roles made clear. Each config declared, each volume in place, Through Compose they unite, to streamline the space. Compose pathways, structured and strong, Linking apps to environments, where they belong. Bound by one purpose, to simplify all, Empowering development, answering the call.

## Version Catalog

Control all dependencies from a central location. GitHub [Hlæja Version Catalog](https://github.com/swordsteel/hlaeja-version-catalog)

## Gradle Plugin

### Core Plugin

Plugin containing basic function ust in all repositories. GitHub [Hlæja Core Plugin](https://github.com/swordsteel/hlaeja-core-plugin)

### Common Plugin

Plugin containing gradle task and setting used by common, library, and service repositories. GitHub [Hlæja Common Plugin](https://github.com/swordsteel/hlaeja-common-plugin)

## Library

### Common Messages

Library containing all internal messages for services. GitHub [Hlæja Common Messages](https://github.com/swordsteel/hlaeja-common-messages)

## Services

### Device Data

Store measurement from electronic devices. GitHub [Hlæja Device Data](https://github.com/swordsteel/hlaeja-device-data)

#### Environment

```text
SPRING_PROFILES_ACTIVE: docker
INFLUXDB_TOKEN: influxdbToken==
```

### Device Registry

Store device information. GitHub [Hlæja Device Data](https://github.com/swordsteel/hlaeja-device-registry)

#### Environment

```text
SPRING_R2DBC_URL: r2dbc:postgresql://localhost:5432/device_registry
SPRING_R2DBC_USERBAME: services
SPRING_R2DBC_PASSWORD: password
JWT_PRIVATE_KEY: keys/private_key.pem
```

#### Volume

This will allow you to mount a local private key `identity_private_key.pem` into the container. Read `IDENTITY.md` for how to generate.

```text
volumes:
  - ./keys/identity_private_key.pem:/app/resources/keys/private_key.pem
```

### Device API

Api for electronic devices. GitHub [Hlæja Device Data](https://github.com/swordsteel/hlaeja-device-api)

#### Environment

```text
SPRING_PROFILES_ACTIVE: docker
```

#### Volume

This will allow you to mount a local keystore `device_api_keystore.p12`, and local public key `identity_public_key.pem` into the container. Read `CERTIFICATE.md`, and `IDENTITY.md` for how to generate.

```text
volumes:
  - ./keys/identity_public_key.pem:/app/resources/cert/public_key.pem
  - ./keys/device_api_keystore.p12:/app/resources/cert/keystore.p12
```

## Databases

### InfluxDB

InfluxDB is a high-performance time series database designed to handle large volumes of time-stamped data. It is commonly used for monitoring, analytics, and IoT applications, where data points are associated with timestamps (e.g., sensor readings, system metrics).

#### Environment

```text
DOCKER_INFLUXDB_INIT_MODE: setup
DOCKER_INFLUXDB_INIT_USERNAME: influx
DOCKER_INFLUXDB_INIT_PASSWORD: password
DOCKER_INFLUXDB_INIT_ADMIN_TOKEN: influxdbToken==
DOCKER_INFLUXDB_INIT_ORG: hlaeja_ltd
DOCKER_INFLUXDB_INIT_BUCKET: device-data
```

### PostgreSQL

PostgreSQL is a powerful, open-source relational database management system (RDBMS). Known for its reliability and advanced features, it supports SQL for querying and managing data, along with extensive functionality for scalability and extensibility.

#### Environment

```text
POSTGRES_USER: postgres
POSTGRES_PASSWORD : password
```

### PostgreSQL

Apache Cassandra is a distributed NoSQL database designed for handling large amounts of data across many commodity servers with no single point of failure. It is optimized for high availability, scalability, and fault tolerance.

#### Environment

```text
CASSANDRA_USER: cassandra
CASSANDRA_PASSWORD: password
```
