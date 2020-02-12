# Standard Keptn 0.6.0 User Story

*Last update: 12.02.2020*

This use story shows the standard flow as implemented in Keptn 0.6.0, but based on the new shipyard and uniform specification.

## User view

:man: *Tim:* I'm a developer and responsible for building the frontend service of the app *carts*. I'm individually testing my service in a *dev* environment to investigate bugfixes and new features. After successful tests, a new version of a service should be automatically promoted to the hardening stage. 

### Initial situation

No specific setup, normal project setup (e.g., carts and carts-db) with a normal [shipyard.yaml](../0_basics/shipyard.yaml) and [uniform.yaml](../0_basics/shipyard.yaml).

### Domain events

Tim, I want to release my new version of the carts service: 
```console
keptn trigger configuration-change --project=sockshop
  --service=carts
  --artifact carts:0.10.1
```

### Desired outcome

Tim expects that his service is deployed in *dev* and functional tests are executed. After the test execution, a quality validation is conducted. If the quality validation returns a good result, the new artifact gets deployed in a blue/gree manner into the *hardening* stage where performance tests are executed. If the quality validation in hardening returns a good result, the new version gets promoted into *produciton*. 

## Event stream

### CD in Dev stage

*Event stream triggered by Tim:* 

- configuration-change.triggered
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
- configuration-change.finished

### CD in Hardening stage

*Event stream continued due to promote action in dev:* 

- configuration-change.triggered
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
- configuration-change.finished

### CD in Production stage

*Event stream continued due to promote action in hardening:* 

- configuration-change.triggered
  - deployment.triggered 
    - deployment.started 
    - deployment.finished
  - test.triggered
    - test.started
    - test.finished
  - evaluation.triggered
    - evaluation.started
    - evaluation.finished
  - approval.triggered
    - approval.started
    - approval.finished
  - release.triggered
    - release.started
    - release.finished
- configuration-change.finished

### Problem detected

*Event triggered by problem in production:* 

- problem-open.triggered
  - remediation.triggered 
    - remediation.started 
    - remediation.finished
  - test.triggered
    - test.started
    - test.finished
  - evaluation.triggered
    - evaluation.started
    - evaluation.finished
- problem-open.finished