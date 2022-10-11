# fabos-aas-stack-sample

Docker-Compose-Stack mit einem [BaSyx Registry Container](https://wiki.eclipse.org/BaSyx_/_Documentation_/_Components_/_Registry) und einem [BaSyx AAS Server](https://wiki.eclipse.org/BaSyx_/_Documentation_/_Components_/_AAS_Server), der eine Kamera AAS (`camera_model.aasx`) bereitstellt. (Siehe <https://github.com/eclipse-basyx/basyx-java-examples/blob/main/basyx.docker/simple-deployment/docker-compose.yml>)

## Setup

`docker compose up` startet

- Registry <http://localhost:4000/registry/api/v1/registry> ([API auf Swaggerhub](https://app.swaggerhub.com/apis/BaSyx/BaSyx_Registry_API/v1)) und
- Server <http://localhost:4001/aasServer/shells/> ([API auf Swaggerhub](https://app.swaggerhub.com/apis/BaSyx/basyx_asset_administration_shell_repository_http_rest_api/v1))

## Beispiel

Läuft die Kamera `IDSCamAAS001`?

```
    curl http://localhost:4001/aasServer/shells/IDSCamAAS001/aas/submodels/OperationalData/submodel/submodelElements/Running/value
```

```
    false
```
