# Hlæja Development

Services and networks, to shape and to steer, Containers in harmony, their roles made clear. Each config declared, each volume in place, Through Compose they unite, to streamline the space. Compose pathways, structured and strong, Linking apps to environments, where they belong. Bound by one purpose, to simplify all, Empowering development, answering the call.

## Setup 

### Databases

Hlæja using different databases read [Database setup](./doc/docker_database.md)

### Hlæja Services

Hlæja consists of services read [service setup](./doc/docker_hlaeja.md)

## Repositories

Hlæja is a system build from Gradle plugins, libraries, and services, look at [dependencies](./doc/dependency.md) visualisation

### Version Catalog

Control all dependencies from a central location. GitHub [Hlæja Version Catalog](https://github.com/swordsteel/hlaeja-version-catalog)

### Gradle Plugin

#### Core Plugin

Plugin containing basic function ust in all repositories. GitHub [Hlæja Core Plugin](https://github.com/swordsteel/hlaeja-core-plugin)

#### Common Plugin

Plugin containing gradle task and setting used by common, library, and service repositories. GitHub [Hlæja Common Plugin](https://github.com/swordsteel/hlaeja-common-plugin)

### Library

#### Common Messages

Library containing all internal messages for services. GitHub [Hlæja Common Messages](https://github.com/swordsteel/hlaeja-common-messages)

#### Common JWT

Library containing JWT for services. GitHub [Hlæja JWT](https://github.com/swordsteel/hlaeja-jwt)

### Services

#### Device Data

Store measurement from electronic devices. GitHub [Hlæja Device Data](https://github.com/swordsteel/hlaeja-device-data)

#### Device Registry

Store device information. GitHub [Hlæja Device Data](https://github.com/swordsteel/hlaeja-device-registry)

#### Device API

Api for electronic devices. GitHub [Hlæja Device Data](https://github.com/swordsteel/hlaeja-device-api)

#### Device Configuration

Store configurations for electronic devices. GitHub [Hlæja Device Configuration](https://github.com/swordsteel/hlaeja-device-configuration)

#### Registry API

API for register devices when flashed. GitHub [Hlæja Registry API](https://github.com/swordsteel/hlaeja-registry-api)

#### Account Registry

Store Information of accounts. GitHub [Hlæja Account Registry](https://github.com/swordsteel/hlaeja-account-registry)
