# Hlæja K8s

## Table of Contents

<!-- TOC -->
* [Hlæja K8s](#hlæja-k8s)
  * [Table of Contents](#table-of-contents)
  * [Initialize](#initialize)
    * [Namespace](#namespace)
    * [Registry Secret](#registry-secret)
    * [JSON Web Token (JWT)](#json-web-token-jwt)
  * [Databases](#databases)
    * [Postgres](#postgres)
      * [Secret](#secret)
      * [Config Map](#config-map)
      * [Stateful Set](#stateful-set)
      * [Service](#service)
  * [Hlæja](#hlæja)
    * [Account Register](#account-register)
      * [Secret](#secret-1)
      * [Config Map](#config-map-1)
      * [Deployment](#deployment)
      * [Service](#service-1)
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

## Hlæja

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
 
