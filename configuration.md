# Configuration

## Overview

Configuration for each service is done primarily through environment variables:

```bash
PORT: 3000
DATABASE_NAME: postgres
DATABASE_USER: postgres
DATABASE_PASSWORD: postgres
DATABASE_HOST: postgres
DATABASE_PORT: 5432
```

### Reading the configuration

The service should define a configuration type to hold all the configuration values needed. Those values can then be loaded when the service boots

```typescript
export interface Config {
    port: number;
    knex: KnexConfig;
};

const appConfig: Config = {
    port: Number(process.env.PORT || 3000),
    knex: {
        client: 'pg',
        connection: {
            host: process.env.DATABASE_HOST,
            database: process.env.DATABASE_NAME,
            password: process.env.DATABASE_PASSWORD,
            user: process.env.DATABASE_USER,
            port: Number(process.env.DATABASE_PORT),
        },
    },
};


export default appConfig;
```

In some cases, a sensible value can be set if not provided by the environment

### Docker Compose

When running with docker compose, use the `environments` section of the service to set the values

```yaml
services:
  submission:
    environment:
      PORT: 3000
      DATABASE_NAME: postgres
      DATABASE_USER: postgres
      DATABASE_PASSWORD: postgres
      DATABASE_HOST: postgres
      DATABASE_PORT: 5432
...
```

### Kubernetes

In a `Deployment` set the environment variables for each container

```yaml
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
          env:
            - name: DATABASE_NAME
              value: "{{ include "libero-reviewer.fullname" . }}-postgresql"
            - name: DATABASE_PORT
              value: "5432"
            - name: DATABASE_USER
              value: "{{ .Values.postgresql.postgresqlUsername }}"
            - name: DATABASE_PASSWORD
              value: "{{ .Values.postgresql.postgresqlPassword }}"
            - name: DATABASE_NAME
              value: "{{ .Values.postgresql.postgresqlDatabase }}"

```

## Service settings

### Submission

The submission service uses the following configuration values

`DATABASE_NAME`: postgres database name  
`DATABASE_USER`: postgres username  
`DATABASE_PASSWORD`: postgres password  
`DATABASE_HOST`: postgres hostname  
`DATABASE_PORT`: postgres access port

`S3_AWS_ENDPOINT`: S3 service endpoint, only used for testing and dev  
`S3_ACCESS_KEY_ID`: AWS access key id for S3  
`S3_SECRET_ACCESS_KEY`: Aws secrety key for S3  
`S3_FORCE_PATH_STYLE`: Always set to 'true'  
`S3_FILE_BUCKET`: Name of the S3 Bucket for file uploads

`NEW_RELIC_HOME`: local path to newrelic.js config file

`MAX_QL_COMPLEXITY`: maximum GraphQL complexity \(recommended: 100\)  
`MAX_QL_DEPTH`: maximum GraphQL query depth \(recommended: 5\)  
`MAX_FILE_SIZE_IN_BYTES`: maximum file upload

`AUTHENTICATION_JWT_SECRET`: Libero reviewer authentication secret  
`USER_ADAPTER_URL`: Continuum Adaptor endpoint for getting current user info \(http://continuum-adaptor.url/current-user\).   
`SCIENCE_BEAM_URL`: ScienceBeam endpoint for converting uploaded manuscript, for example`http://continuum-api.url/science-beam/convert.` \(from continuum\)  
`SCIENCE_BEAM_TIMEOUT`: Conversion timeout value \(recommended: 20000\)

### Continuum Adaptor

`PORT`: The listening port for the service  
`RABBITMQ_URL`: The url for rabbitmq.  
`LOGIN_URL`: The url for continuum \(e.g. https://elifesciences.org/submit\)  
`LOGIN_RETURN_URL`: The login url to return to after token exchange \(e.g. https://reviewer.elifesciences.org/login\)  
  
`DATABASE_NAME`: postgres database name  
`DATABASE_USER`: postgres username  
`DATABASE_PASSWORD`: postgres password  
`DATABASE_HOST`: postgres hostname  
`DATABASE_PORT`: postgres access port  
  
`AUTHENTICATION_JWT_SECRET`: Libero reviewer authentication secret  
`CONTINUUM_JWT_SECRET`: The secret for continuum journal \(from continuum\)  
`CONTINUUM_API_URL`: Continuum Api endpoint \(from continuum\)  
`ELIFE_API_GATEWAY_SECRET`: Elife Api Gateway secret \(from continuum\)



