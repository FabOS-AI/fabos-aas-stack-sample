# fabos-aas-stack-sample

Docker-Compose-Stack consisting of a [BaSyx Registry Container](https://wiki.eclipse.org/BaSyx_/_Documentation_/_Components_/_Registry) and a [BaSyx AAS Server](https://wiki.eclipse.org/BaSyx_/_Documentation_/_Components_/_AAS_Server) which serves a camera aas (`config/camera_model.aasx`).

See also: <https://github.com/eclipse-basyx/basyx-java-examples/blob/main/basyx.docker/simple-deployment/docker-compose.yml>

## Requirements

- [Docker](https://www.docker.com/)

## Usage

Start stack:

    docker compose up -d

This makes the containers available:

- Registry
  - <http://localhost:4000/registry/api/v1/registry>
  - [API on Swaggerhub](https://app.swaggerhub.com/apis/BaSyx/BaSyx_Registry_API/v1)
- Server
  - <http://localhost:4001/aasServer/shells/>
  - [API on Swaggerhub](https://app.swaggerhub.com/apis/BaSyx/basyx_asset_administration_shell_repository_http_rest_api/v1)

## Example

The requests in this example are defined as curl commands and are also avaialble in the public [FabOS Postman workspace](https://www.postman.com/fabos-ai/workspace/service-lifecycle-management/collection/22732344-6cc866d8-8889-4192-ba3f-d1a02f411e46?ctx=documentation).

We want to know if a camera is currently running. The camera has a connected AAS with the id `IDSCamAAS001`, so let’s send a request to the registry:

    curl http://localhost:4000/registry/api/v1/registry/IDSCamAAS001

We get a JSON response that tells us the endpoints of the AAS and its submodels:

``` json
{
    "modelType": {
        "name": "AssetAdministrationShellDescriptor"
    },
    "idShort": "IDSCamAAS",
    "identification": {
        "idType": "Custom",
        "id": "IDSCamAAS001"
    },
    "endpoints": [
        {
            "type": "http",
            "address": "http://localhost:4001/aasServer/shells/IDSCamAAS001/aas"
        }
    ],
    // ...
    "submodels": [
        {
            // ...
            "idShort": "OperationalData",
            // ...
            "endpoints": [
                {
                    "type": "http",
                    "address": "http://localhost:4001/aasServer/shells/IDSCamAAS001/aas/submodels/OperationalData/submodel"
                }
            ],
            // ...
        },
        {
            // ...
            "idShort": "Identification",
            // ...
            "endpoints": [
                {
                    "type": "http",
                    "address": "http://localhost:4001/aasServer/shells/IDSCamAAS001/aas/submodels/Identification/submodel"
                }
            ],
            // ...
        },
        {
            // ...
            "idShort": "TechnicalData",
            // ...
            "endpoints": [
                {
                    "type": "http",
                    "address": "http://localhost:4001/aasServer/shells/IDSCamAAS001/aas/submodels/TechnicalData/submodel"
                }
            ],
            // ...
        }
    ]
}
```

Let’s check the provided information of the `OperationalData` submodel by sending a request to its endpoint:

    curl http://localhost:4001/aasServer/shells/IDSCamAAS001/aas/submodels/OperationalData/submodel

Again, we get a JSON response, this time from the AAS server:

``` json
{
    "idShort": "OperationalData",
    "identification": {
        "idType": "IRI",
        "id": "https://example.com/ids/sm/5235_7040_0112_9419"
    },
    "dataSpecification": [],
    "embeddedDataSpecifications": [],
    "modelType": {
        "name": "Submodel"
    },
    "kind": "Instance",
    "submodelElements": [
        {
            "modelType": {
                "name": "Property"
            },
            "kind": "Instance",
            "value": false,
            "valueType": "boolean",
            "idShort": "Running",
            "category": "VARIABLE",
            "qualifiers": [],
            "semanticId": {
                "keys": []
            },
            "parent": {
                "keys": [
                    {
                        "type": "Submodel",
                        "local": true,
                        "value": "https://example.com/ids/sm/5235_7040_0112_9419",
                        "idType": "IRI"
                    }
                ]
            }
        },
        // ...
    ],
    // ...
}
```

We can see that the item in `submodelElements` with the `idShort` `"Running"` has the `value` `"false"`.

We can also directly access this value:

    curl http://localhost:4001/aasServer/shells/IDSCamAAS001/aas/submodels/OperationalData/submodel/submodelElements/Endpoint/value

Response:

``` json
"http://10.32.170.98:801/mjpeg"
```

Or get all values of the submodel:

    curl http://localhost:4001/aasServer/shells/IDSCamAAS001/aas/submodels/OperationalData/submodel/values

Response:

``` json
{
    "Running":false,
    "Error":"0",
    "Status":"701",
    "Exposure": 
    {
        "Max":0,
        "Min":0,
        "Step":0,
        "Value":0
    },
    "Endpoint":"http://10.32.170.98:801/mjpeg"
}
```
