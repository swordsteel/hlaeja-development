# Hlæja K8s

## Table of Contents

<!-- TOC -->
* [Hlæja K8s](#hlæja-k8s)
  * [Table of Contents](#table-of-contents)
  * [Initialize](#initialize)
    * [Namespace](#namespace)
    * [Registry Secret](#registry-secret)
    * [JSON Web Token (JWT)](#json-web-token-jwt)
    * [Keystore](#keystore)
  * [Databases](#databases)
    * [Postgres](#postgres)
      * [Secret](#secret)
      * [Config Map](#config-map)
      * [Stateful Set](#stateful-set)
      * [Service](#service)
    * [Cassandra](#cassandra)
      * [Stateful Set](#stateful-set-1)
      * [Service](#service-1)
    * [InfluxDb](#influxdb)
      * [Secret](#secret-1)
      * [Config Map](#config-map-1)
      * [Stateful Set](#stateful-set-2)
      * [Service](#service-2)
    * [Redis](#redis)
      * [Stateful Set](#stateful-set-3)
      * [Service](#service-3)
  * [Hlæja](#hlæja)
    * [Account Register](#account-register)
      * [Secret](#secret-2)
      * [Config Map](#config-map-2)
      * [Deployment](#deployment)
      * [Service](#service-4)
    * [Device Register](#device-register)
      * [Secret](#secret-3)
      * [Config Map](#config-map-3)
      * [Deployment](#deployment-1)
      * [Service](#service-5)
    * [Device Configuration](#device-configuration)
      * [Secret](#secret-4)
      * [Config Map](#config-map-4)
      * [Deployment](#deployment-2)
      * [Service](#service-6)
    * [Device Data](#device-data)
      * [Secret](#secret-5)
      * [Config Map](#config-map-5)
      * [Deployment](#deployment-3)
      * [Service](#service-7)
    * [Device API](#device-api)
      * [Config Map](#config-map-6)
      * [Deployment](#deployment-4)
      * [Service](#service-8)
<!-- TOC -->

----

## Initialize

### Namespace

Create the Namespace for the environment.

```bash
kubectl apply -f .\kube\01-initialize\01-namespace.yaml
```

---

### Registry Secret

Create repository secret

```bash
kubectl apply -f .\kube\01-initialize\02-registry-secret.yaml
```

**How to make JSON Configuration**

```json=
{
  "auths": {
    "<your-registry>": {
      "username": "<your-username>",
      "password": "<your-password>",
      "email": "<your-email@example.com>",
      "auth": "<base64-of-your-username:your-password>"
    }
  }
}
```

**Replace Values**

- **Replace** <your-registry>: Use the hostname of your Gitea instance (e.g., registry.example.com).
- **Replace** <your-username>: Use your Gitea username (e.g., user1).
- **Replace** <your-password>: Use your Gitea personal access token generated with read:package scope (e.g., abc123).
- **Replace** <your-email>: Use your email address (e.g., user1@example.com).

**Linux Command**

```bash
echo -n 'your-username:your-password' | base64 -w 0
```

witch gives `eW91ci11c2VybmFtZTp5b3VyLXBhc3N3b3Jk` then we use it in the `auth`

```bash
echo -n '{"auths":{"<your-registry>":{"username":"your-username","password":"your-password","email":"your-email","auth":"eW91ci11c2VybmFtZTp5b3VyLXBhc3N3b3Jk"}}}' | base64 -w 0
```

witch give `eyJhdXRocyI6eyI8eW91ci1yZWdpc3RyeT4iOnsidXNlcm5hbWUiOiJ5b3VyLXVzZXJuYW1lIiwicGFzc3dvcmQiOiJ5b3VyLXBhc3N3b3JkIiwiZW1haWwiOiJ5b3VyLWVtYWlsIiwiYXV0aCI6ImVXOTFjaTExYzJWeWJtRnRaVHA1YjNWeUxYQmhjM04zYjNKayJ9fX0=`

---

### JSON Web Token (JWT)

For JWT we are using public and private keys, read more about [RSA keys](./rsa_key.md).

Account private key for account service to make access token.

```bash
kubectl apply -f .\kube\01-initialize\03-account-jwt-private-key-secret.yaml
```

Account public key for all services identifying users

```bash
kubectl apply -f .\kube\01-initialize\04-account-jwt-public-key-secret.yaml
```

Device private key for device service to make device token.

```bash
kubectl apply -f .\kube\01-initialize\05-device-jwt-private-key-secret.yaml
```

Device public key for all services identifying devices

```bash
kubectl apply -f .\kube\01-initialize\06-device-jwt-public-key-secret.yaml
```

---

### Keystore

Keystore with password read more about [Keystore.p12](./keystore.md).

check cert:

```
keytool -list -v -storetype PKCS12 -keystore keystore.p12 -storepass <password>
```

option:

```
kubectl create secret generic <name> \
  --from-file=keystore.p12=<keystore.p12> \
  --from-literal=keystore-password=<your-keystore-password> \
  -n <namespace>
```

Device API Keystore
```bash
kubectl apply -f .\kube\01-initialize\07-device-api-keystore.yaml
```

---

## Databases

### Postgres

Remember that you don't run replicas but many instances with its own storage and service.

#### Secret

```bash
kubectl apply -f .\kube\02-databases\01-postgres\01-secret.yaml
```

Set values:

- postgres root password

using something a bit more secure `SCRAM-SHA-256$4096:f/IWlCTGdMT9qOjQlPbWtA==$qePy5ArW+7ykg3yHqW7qYH0j2384OIoV2IcBcz0mIRM=:KuU1xgnAVtOVpCZhdUJlI8F7Viz0ApmYxYEo5yXNCW0=` in this case we use `password`, to make this... use postgres to make a user and password, copy this value and now will use as admin password.

#### Config Map

```bash
kubectl apply -f .\kube\02-databases\01-postgres\02-configmap.yaml
```

Set values:

- postgres root user

#### Stateful Set

This is the specifications for postgres.

```bash
kubectl apply -f .\kube\02-databases\01-postgres\03-statefulset.yaml
```

Set storage size for permanent storage

#### Service

this exposes port and ip.

```bash
kubectl apply -f .\kube\02-databases\01-postgres\04-service.yaml
```

---

### Cassandra

For now... run basic cassandra, we need to add authentication later.

to get a clean cassandra configuration:

```bash
docker run --rm cassandra:5.0 cat /etc/cassandra/cassandra.yaml > cassandra-default.yaml
```

modify `authenticator` and `authorizer` and som how get that change inside... local file get to big 262144 bytes limitation.

some help things for later

```bashe
kubectl exec -it -n hlaeja cassandra-0 -- bash
```

run one of this

```bash
nodetool status
```

or

```bash
cqlsh
SELECT data_center FROM system.local;
```

#### Stateful Set

This is the specifications for cassandra.

```bash
kubectl apply -f .\kube\02-databases\02-cassandra\01-statefulset.yaml
```

Set storage size for permanent storage

#### Service

this exposes port and ip.

```bash
kubectl apply -f .\kube\02-databases\02-cassandra\02-service.yaml
```

---

### InfluxDb

#### Secret

```bash
kubectl apply -f .\kube\02-databases\03-influxdb\01-secret.yaml
```

Set values:

- influx root password
- influx token

using something a bit more secure `SCRAM-SHA-256$4096:f/IWlCTGdMT9qOjQlPbWtA==$qePy5ArW+7ykg3yHqW7qYH0j2384OIoV2IcBcz0mIRM=:KuU1xgnAVtOVpCZhdUJlI8F7Viz0ApmYxYEo5yXNCW0=` in this case we use `password`, to make this... use postgres to make a user and password, copy this value and now will use as admin password.

#### Config Map

```bash
kubectl apply -f .\kube\02-databases\03-influxdb\02-configmap.yaml
```

Set values:

- influx root username
- influx mode
- influx organisation
- influx bucket

#### Stateful Set

This is the specifications for influxdb.

```bash
kubectl apply -f .\kube\02-databases\03-infulxdb\03-statefulset.yaml
```

Set storage size for permanent storage

#### Service

this exposes port and ip.

```bash
kubectl apply -f .\kube\02-databases\03-infulxdb\04-service.yaml
```

---

### Redis

For now... run basic redis, we need to add authentication, replication later? need to think mor about this later.

#### Stateful Set

This is the specifications for redis.

```bash
kubectl apply -f  .\kube\02-databases\04-redis\01-statefulset.yaml
```

Set storage size for permanent storage.

did add storage for restarts and some limits.

#### Service

this exposes port and ip.

```bash
kubectl apply -f .\kube\02-databases\04-redis\02-service.yaml
```

---

## Hlæja

To access service use `kubectl exec -it <pod-name> -n hlaeja -- /bin/sh`

To tail a service log use `kubectl logs -f <pod-name> -n hlaeja`

### Account Register

This is only a ***concept*** and exist for testing rest of system. this need to be ***rewritten***.

#### Secret

```bash
kubectl apply -f .\kube\03-hlaeja\01-account-registry\01-secret.yaml
```

Set values:

- postgres password

#### Config Map

```bash
kubectl apply -f .\kube\03-hlaeja\01-account-registry\02-configmap.yaml
```

Set values:

- spring profile
- postgres username
- postgres url
- account private jwt file location

#### Deployment

Account Registry Service, using `account-jwt-private-key`

```bash
kubectl apply -f .\kube\03-hlaeja\01-account-registry\03-deployment.yaml
```

#### Service

this service should not be accessible from world only open in testing

```bash
kubectl apply -f .\kube\03-hlaeja\01-account-registry\04-service.yaml
```

---

### Device Register

#### Secret

```bash
kubectl apply -f .\kube\03-hlaeja\02-device-registry\01-secret.yaml
```

Set values:

- postgres password

#### Config Map

```bash
kubectl apply -f .\kube\03-hlaeja\02-device-registry\02-configmap.yaml
```

Set values:

- spring profile
- postgres username
- postgres url
- device private jwt file location

#### Deployment

Account Registry Service, using `account-jwt-private-key`

```bash
kubectl apply -f .\kube\03-hlaeja\02-device-registry\03-deployment.yaml
```

#### Service

this service should not be accessible from world only open in testing

```bash
kubectl apply -f .\kube\03-hlaeja\02-device-registry\04-service.yaml
```

---

### Device Configuration

#### Secret

```bash
kubectl apply -f .\kube\03-hlaeja\03-device-configuration\01-secret.yaml
```

Set values:

- cassandra password (db have not turned this on yet)

#### Config Map

```bash
kubectl apply -f .\kube\03-hlaeja\03-device-configuration\02-configmap.yaml
```

Set values:

- spring profile
- cassandra username (db have not turned this on yet)
- cassandra contact points

#### Deployment

```bash
kubectl apply -f .\kube\03-hlaeja\03-device-configuration\03-deployment.yaml
```

#### Service

this service should not be accessible from world only open in testing

```bash
kubectl apply -f .\kube\03-hlaeja\03-device-configuration\04-service.yaml
```

---

### Device Data

#### Secret

```bash
kubectl apply -f .\kube\03-hlaeja\04-device-data\01-secret.yaml
```

Set values:

- influxdb token

#### Config Map

```bash
kubectl apply -f .\kube\03-hlaeja\04-device-data\02-configmap.yaml
```

Set values:

- spring profile
- influxdb url

#### Deployment

```bash
kubectl apply -f .\kube\03-hlaeja\04-device-data\03-deployment.yaml
```

#### Service

this service should not be accessible from world only open in testing

```bash
kubectl apply -f .\kube\03-hlaeja\04-device-data\04-service.yaml
```

---

### Device API

#### Config Map

```bash
kubectl apply -f .\kube\03-hlaeja\05-device-api\01-configmap.yaml
```

Set values:

- spring profile
- spring data redis database
- spring data redis host
- device configuration url
- device data url
- device register url

#### Deployment

```bash
kubectl apply -f .\kube\03-hlaeja\05-device-api\02-deployment.yaml
```

#### Service

this service should not be accessible from world only open in testing

```bash
kubectl apply -f .\kube\03-hlaeja\05-device-api\03-service.yaml
```
