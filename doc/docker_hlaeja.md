# Hlæja Services

## Device Data

### Environment

```text
SPRING_PROFILES_ACTIVE: docker
INFLUXDB_TOKEN: influxdbToken==
```

## Device Registry

### Environment

```text
SPRING_R2DBC_URL: r2dbc:postgresql://localhost:5432/device_registry
SPRING_R2DBC_USERBAME: services
SPRING_R2DBC_PASSWORD: password
JWT_PRIVATE_KEY: cert/private_key.pem
```

### Volume

Mount a local private key into the container. Read [`rsa_key.md`](./rsa_key.md) for how to generate.

```text
volumes:
  - ./cert/device_private_key.pem:/app/resources/cert/private_key.pem
```

## Device API

### Environment

```text
SPRING_PROFILES_ACTIVE: docker
```

### Volume

Mount a local public key into the container. Read [rsa_key.md](./rsa_key.md) for how to generate.

Mount a local keystore into the container. Read [keystore.md](./keystore.md) for how to generate.

```text
volumes:
  - ./cert/device_public_key.pem:/app/resources/cert/public_key.pem
  - ./cert/device_api_keystore.p12:/app/resources/cert/keystore.p12
```

## Device Configuration

### Environment

```text
SPRING_PROFILES_ACTIVE: docker
```

## Registry API

### Environment

```text
SPRING_PROFILES_ACTIVE: docker
```

### Volume

Mount a local public key into the container. Read [rsa_key.md](./rsa_key.md) for how to generate.

Mount a local keystore into the container. Read [keystore.md](./keystore.md) for how to generate.

```text
volumes:
  - ./cert/account_public_key.pem:/app/resources/cert/public_key.pem
  - ./cert/registry_api_keystore.p12:/app/resources/cert/keystore.p12
```

## Account Registry

### Environment


```text
SPRING_R2DBC_URL: r2dbc:postgresql://localhost:5432/account_registry
SPRING_R2DBC_USERBAME: services
SPRING_R2DBC_PASSWORD: password
JWT_PRIVATE_KEY: cert/private_key.pem
```

### Volume

Mount a local private key into the container. Read [`rsa_key.md`](./rsa_key.md) for how to generate.

```text
volumes:
  - ./cert/account_private_key.pem:/app/resources/cert/private_key.pem
```

## Management

### Environment

```text
SPRING_PROFILES_ACTIVE: docker
```

### Volume

Mount a local public key into the container. Read [rsa_key.md](./rsa_key.md) for how to generate.

```text
volumes:
  - ./cert/account_public_key.pem:/app/resources/cert/public_key.pem
```
