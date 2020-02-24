# Standard Keptn 0.6.0 User Story

*Last update of shipyard and uniform: 17.02.2020*

As a user, I want to sync a stage with the last successfully evaluated configuration of its preceding stage.

**Note:** This user story has a strong relation to: [Release Brackets](release_brackets/)

## User view

:man: *Tim:* I'm a developer and responsible for building the frontend service of the app *carts*. I'm individually testing my service in a *dev* environment to investigate bugfixes and new features.

:blonde_woman: *Sue*: I'm a performance engineer and responsible for testing our application in the hardening stage. Regularly, I check whether the *hardening* stage is out-of-sync with its preceding stage, the *dev* stage. If it is, I manually trigger a sync mechanism. 

### Initial situation

Normal project setup as shown in [shipyard.yaml](./shipyard.yaml) and tooling is defined in [uniform.yaml](./uniform.yaml).

*Hardening stage:*
- All services are configured to run with a replica of 3 to allow rolling updates.

*Production stage:*
- When performance tests are passed in hardening, new services should get promoted to *production* automatically. 

### Domain events

**Sue synchronizes dev with hardening**

Sue, checks the status of the *hardening* stage: 

```console
keptn view status --stage=hardening --project=sockshop
```
```
> Released services in hardening:
- carts:0.10.1
- carts-db:0.4.0
- catalogue:1.7.0

> Ongoing deployments/tests/evaluations:
- N/A

> Sync status: hardening is OUT-OF-SYNC with dev. 
Last successfully evaluated configuration:
- carts:0.10.17
- carts-db:0.4.1
- catalogue:1.7.0

> Execute: `keptn trigger artifact-delivery --project=foo --stage=hardening` to update: 
- carts:0.10.1 --> carts:0.10.17
- carts-db:0.4.0 --> carts-db:0.4.1
```

Sue, I want to trigger the synchronization of *hardening* with *dev*: 
```console
keptn trigger artifact-delivery 
  --project=foo
  --stage=hardening
  (--from=dev         # smart default: per default always a synchronization with the preceding stage)  
```

**Keptn synchronizes hardening and production**

TBD

### Desired outcome

Sue expects that the last successfully evaluated configuration of the *dev* stage is taken over to the *hardening* stage. Then, automatically the actions - as specified in the shipyard for stage *hardening* - get executed.

## Event stream

### CD in Dev stage

*Event stream triggered by Tim:* 

- deployment.triggered 
  - deployment.started
  - deployment.finished
- test.triggered
  - test.started
  - test.finished
- evaluation.triggered
  - evaluation.started
  - evaluation.finished
- release.triggered
  - release.started
  - release.finished

### CD in Hardening stage

*Event stream triggerd by Sue's sync:* 

- deployment.triggered 
  - deployment.started 
  - deployment.finished
- test.triggered
  - test.started
  - test.finished
- test.triggered
  - test.started
  - test.finished
- evaluation.triggered
  - evaluation.started
  - evaluation.finished
- release.triggered
  - release.started
  - release.finished

### CD in Production stage

*Event stream triggerd by Keptn`s sync:* 

- update.triggered
  - update.started 
  - update.finished
- deployment.triggered 
  - deployment.started 
  - deployment.finished
- test.triggered
  - test.started
  - test.finished
- test.triggered
  - test.started
  - test.finished
- evaluation.triggered
  - evaluation.started
  - evaluation.finished
- release.triggered
  - release.started
  - release.finished