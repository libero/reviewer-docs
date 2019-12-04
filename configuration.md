# Configuration

## Summary

Configuration values for each service are provided  via a `config.json` that is read by the main process at startup. The file should reside in a top level `config` folder. An example file `config.example.json` should be provided in that folder to serve as a quick start for local development.

The location of the configuration file is determined by the `CONFIG_PATH` environment variable which defaults to `/etc/reviewer/config.json`.

## Running locally on host

When running locally via `yarn run start:dev`,  the  `CONFIG_PATH` environment variable is overridden to `./config/config.js` as part of the command definition in the `package.json` file:

```javascript
{
  scripts: {
    ...
    "start:dev": "CONFIG_PATH=config/config.json nodemon --watch 'src/**/*.ts' --ignore 'src/**/*.spec.ts' --exec 'ts-node' src/main.ts | pino-pretty"
  }
}
...
```

## Running locally in docker compose

In the `docker-compose.yml` file, the `config` folder should should be mounted into `/etc/reviewer` folder.

```yaml
...
    volumes:
      - ./:/src/:z
      - ./config/:/etc/reviewer/:z
...
```

## Running on Kubernetes

Its recommended to use a `ConfigMap` resource to generate the file:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ include "libero-reviewer.fullname" . }}-submission
  labels:
    app.kubernetes.io/name: {{ include "libero-reviewer.name" . }}
    helm.sh/chart: {{ include "libero-reviewer.chart" . }}
    app.kubernetes.io/instance: {{ .Release.Name }}
    app.kubernetes.io/managed-by: {{ .Release.Service }}
data:
  config.json: |
    {
      "port": 3000,
      ...
    }
```

The `ConfigMap` should then be mounted in the deployment template for the service:

```text
apiVersion: apps/v1beta2
kind: Deployment
metadata:
  name: {{ include "libero-reviewer.fullname" . }}-submission
  ...
  spec:
    ...
    template:
      spec:
        containers:
          ...
          volumeMounts:
            - name: config-volume
              mountPath: /etc/reviewer
      volumes:
        - name: config-volume
          configMap:
            name: {{ include "libero-reviewer.fullname" . }}-submission

```



