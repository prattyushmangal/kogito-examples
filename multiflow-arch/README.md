# multiflow-arch Project

This project uses Quarkus, the Supersonic Subatomic Java Framework.

If you want to learn more about Quarkus, please visit its website: https://quarkus.io/ .

## Running the application in dev mode

Prereq:
- Running Kafka, can follow Kafka Quickstart
- Create Topic `reader`

You can run your application in dev mode that enables live coding using:
```shell script
./mvnw compile quarkus:dev
```

> **_NOTE:_**  Quarkus now ships with a Dev UI, which is available in dev mode only at http://localhost:8080/q/dev/.

## Packaging and running the application

The application can be packaged using:
```shell script
./mvnw package
```
It produces the `quarkus-run.jar` file in the `target/quarkus-app/` directory.
Be aware that it’s not an _über-jar_ as the dependencies are copied into the `target/quarkus-app/lib/` directory.

The application is now runnable using `java -jar target/quarkus-app/quarkus-run.jar`.

If you want to build an _über-jar_, execute the following command:
```shell script
./mvnw package -Dquarkus.package.type=uber-jar
```

The application, packaged as an _über-jar_, is now runnable using `java -jar target/*-runner.jar`.

## Creating a native executable

You can create a native executable using: 
```shell script
./mvnw package -Pnative
```

Or, if you don't have GraalVM installed, you can run the native executable build in a container using: 
```shell script
./mvnw package -Pnative -Dquarkus.native.container-build=true
```

You can then execute your native executable with: `./target/multiflow-arch-1.0.0-SNAPSHOT-runner`

If you want to learn more about building native executables, please consult https://quarkus.io/guides/maven-tooling.

## User Story Prototype // Executing Workflows
Trying to prototype to match architecture as seen [here](img/MultiworkflowArch.jpg)

Example Kafka Message Consumed to trigger E2E ABC Workflow, create the following message in the `reader` topic
```
{"specversion": "1.0","source": "pm","type": "eventABC","id":"pm","data":{"move": true}}
```

Example Kafka Message Produced
```
{"id":"807aeab0-703b-4687-9717-fcd39736b782","source":"/process/workflowe2e","type":"produceABC","time":"2021-12-23T09:35:03.357942Z","data":{"move":true,"result":"Hello World FROM C!","constant":"Im Constant PM in E2E","response":null,"fromA":"Im in A PM","fromB":"Im in B PM","fromC":"Im in C PM"},"specversion":"1.0","kogitoprocinstanceid":"7d1e08d1-3ef6-4f04-988d-68d9a0ec87bf","kogitoprocid":"workflowe2e","kogitousertaskist":"1"}
{"specversion": "1.0","source": "pm","type": "eventA","id":"pm","data":{"move": true}}
```
--- 

Example Kafka Message Consumed to trigger Subflow, A or B or C Workflow, create the following message in the `reader` topic, changing the Type to trigger the different flows
```
{"specversion": "1.0","source": "pm","type": "eventA","id":"pm","data":{"move": true}}
```

Example Kafka Message Produced
```
"id":"6b634434-15ea-422b-ab10-101e9251349e","source":"/process/workflowA","type":"produceA","time":"2021-12-23T09:37:19.639616Z","data":{"move":true,"result":"Hello World!","fromA":"Im in A PM","response":null},"specversion":"1.0","kogitoprocinstanceid":"b2588924-3e6e-4228-bd9c-8bba9c1d76cb","kogitoprocid":"workflowA","kogitousertaskist":"1"}
```