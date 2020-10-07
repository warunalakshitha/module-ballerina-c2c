# Ballerina Code2cloud Extension
 
Code2cloud extension implementation for ballerina. 

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

## Features:
- Kubernetes deployment support. 
- Kubernetes service support.
- Kubernetes liveness probe support
- Kubernetes readiness probe support
- Kubernetes horizontal pod autoscaler support.
- Docker image generation. 
- Kubernetes secret support.
- Kubernetes config map support.

**Refer [samples](samples) for more info.**

## How to build

1. Download and install JDK 8 or later
2. Install Docker
3. Get a clone or download the source from this repository (https://github.com/ballerinax/kubernetes)
4. Run the Gradle command ``gradle build`` from within the ``module-ballerina-c2c`` directory.
5. Copy ``build/c2c-extension-***.jar`` file to ``<BALLERINA_HOME>/bre/lib`` directory.

### Enabling debug logs
- Use the "BAL_DOCKER_DEBUG=true" environment variable to enable docker related debug logs when building the ballerina
source(s).
- Use the "BAL_KUBERNETES_DEBUG=true" environment variable to enable kubernetes related debug logs when building the 
ballerina source(s).

## Deploy ballerina service directly using `kubectl` command.
This repository also provides a kubectl plugin which allows to build ballerina programs and deploy their kubernetes 
artifacts directly to a kuberetes cluster. The plugin is located at `kubernetes-extension/src/main/resources/kubectl-extension/kubectl-ballerina-deploy`.
Follow the steps mentioned in "[Extend kubectl with plugins](https://kubernetes.io/docs/tasks/extend-kubectl/kubectl-plugins/)"
. Check if the plugin is available using the command `kubectl plugin list`.


### Usage Sample:

```ballerina
import ballerina/http;
import ballerina/log;
import ballerina/c2c as _;

listener http:Listener helloEP = new(9090);

@http:ServiceConfig {
    basePath: "/helloWorld"
}
service helloWorld on helloEP {
    resource function sayHello(http:Caller caller, http:Request request) {
        http:Response response = new;
        response.setTextPayload("Hello, World from service helloWorld ! ");
        var responseResult = caller->respond(response);
        if (responseResult is error) {
            log:printError("error responding back to client.", err = responseResult);
        }
    }
}
```

The kubernetes artifacts will be created in following structure.
```bash
$> tree kubernetes
```
```
kubernetes/
├── docker
│   └── Dockerfile
└── hello-world.yaml
    	
```
