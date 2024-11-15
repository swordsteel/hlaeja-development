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
