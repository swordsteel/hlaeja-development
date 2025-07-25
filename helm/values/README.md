# Hlæja Helm Environment

To make the environment copy `values.yaml` file from `charts/<name>` to `values/<releasesEnviorment>` then change the values you need. or make the file and add the value you like to overwrite.

```
helm/
├── helmfile.yaml
├── values/
│   ├── <releasesEnviorment>/
│   │   ├── <name>.yaml
|   │   └── ...
│   └── ...
└── charts/
    ├── <name>/
    │   ├── Chart.yaml
    │   ├── values.yaml
    │   └── templates/
    │       └── <template>.yaml
    └── ...
```

Then we need to update `helmfile.yaml` one for each environment.

```
releases:
  - name: <releasesName>
    namespace: <releasesEnviorment>
    chart: ./charts/<name>
    values: []

  - name: <releasesName>
    namespace: <releasesEnviorment>
    chart: ./charts/<name>
    values: [./values/<environment>/<name>]

  - ...
```

> **Info:** using default fake base64 values and not specify custom values can break execution.
