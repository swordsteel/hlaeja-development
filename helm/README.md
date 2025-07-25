# Hlæja Helm

## Set up helm environment

how to set up [Enviorment](./values/README.md)

## Deploy to Kubernete

> ⚠️**Warning:** always use `--selector namespace=<releasesEnviorment>` when running `helmfile` or **risk** lose it all!!! ⚠️

> **Info:** limit even more by using `--selector namespace=<releasesEnviorment>,name=<releasesName>`

Create everything for a name space

```shell
helmfile --selector namespace=testing apply
```

Destroy everything for a name space

```shell
helmfile --selector namespace=testing destroy
```

Create initialize for a name space

```shell
helmfile --selector namespace=testing,name=initialize apply
```

Destroy initialize for a name space

```shell
helmfile --selector namespace=testing,name=initialize destroy
```
