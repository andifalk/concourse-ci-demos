# Concourse CI Demo Pipeline
This contains the demo pipeline for Concourse CI

## Requirements

* Java 8 SDK
* Java IDE

## Demo spring boot application

The demo application is just a simple spring boot application providing a REST API
with one GET and one POST request.

```
$ http localhost:9090

HTTP/1.1 200 OK
Cache-Control: no-cache, no-store, max-age=0, must-revalidate
Content-Length: 25
Content-Type: application/json;charset=UTF-8
Expires: 0
Pragma: no-cache

{
    "message": "Hello World"
}
```

```
$ http post localhost:9090 message=world

HTTP/1.1 201 Created
Cache-Control: no-cache, no-store, max-age=0, must-revalidate
Content-Length: 11
Content-Type: application/json;charset=UTF-8
Expires: 0
Pragma: no-cache

Hello world
```

## Concourse Pipelines

The pipelines are created in 3 steps:

1. Basic pipeline just for getting source from git repository and compiling the app and running tests
2. Extend basic pipeline to deploy the created jar file to CloudFoundry (local PCFDev instance)
3. Extend second pipeline with an additional step to upload created binary artifacts into artifactory repository first and then deploy the jar from there to CloudFoundry (PCFDev)

### Basic build pipeline

![Image of basic pipeline](https://github.com/andifalk/concourse-ci-demo/raw/master/images/demo_build_pipeline.png)

```
fly login -t local -c http://127.0.0.1:8080 -u test -p test
fly set-pipeline -t local -p build-demo-app -c ./build-pipeline.yml
```

Initially all pipelines are created in paused state. To unpause the pipeline just use this command:

```
fly unpause-pipeline -t local -p build-demo-app
```

### Build/Deploy pipeline

![Image of build/deploy pipeline](https://github.com/andifalk/concourse-ci-demo/raw/master/images/demo_build_deploy_pipeline.png)

```
fly login -t local -c http://127.0.0.1:8080 -u test -p test
fly set-pipeline -t local -p build-deploy-demo-app -c ./build-deploy-pipeline.yml
```

Initially all pipelines are created in paused state. To unpause the pipeline just use this command:

```
fly unpause-pipeline -t local -p build-deploy-demo-app
```

### Build/Deploy/Scan pipeline

![Image of build/deploy/scan pipeline](https://github.com/andifalk/concourse-ci-demo/raw/master/images/demo_build_deploy_scan.png)

```
fly login -t local -c http://127.0.0.1:8080 -u test -p test
fly set-pipeline -t local -p build-deploy-scan-demo-app -c ./build-deploy-scan-pipeline.yml -l ./credentials.yml
```

Initially all pipelines are created in paused state. To unpause the pipeline just use this command:

```
fly unpause-pipeline -t local -p build-deploy-scan-demo-app
```
