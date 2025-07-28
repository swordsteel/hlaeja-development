# Hlæja Helm

Copy `helmfile.yaml-dev` to `helmfile.yaml` and start to add your environment.

## Set up helm environment

how to set up [Enviorment](./values/README.md)

## Command using kubectl and helmfile

> ⚠️**Warning:** always use `--selector namespace=<releasesEnviorment>` when running `helmfile` or **risk** lose it all!!! ⚠️

> **Info:** limit even more by using `--selector namespace=<releasesEnviorment>,name=<releasesName>`

**Info:** Create everything for a name space

```shell
helmfile --selector namespace=testing apply
```

⚠️**Warning:** Destroy everything for a name space

```shell
helmfile --selector namespace=testing destroy
```

**Info:** Create initialize for a name space

```shell
helmfile --selector namespace=testing,name=initialize apply
```

⚠️**Warning:** Destroy initialize for a name space

```shell
helmfile --selector namespace=testing,name=initialize destroy
```

**Info:** Get status

```shell
kubectl get secret,cm,pvc,pod,svc -n testing
```

⚠️**Warning:** Delete everything!

```shell
kubectl delete ns testing
```
