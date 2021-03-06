# Release Brackets

*Last update of shipyard and uniform: 20.02.2020*

As a user, I want to queue services in staging and finally release them together (even in a canary-way)

## User view

:person_with_blond_hair: *Tim:* I'm a developer and responsible for building the backend service of the app *foo*. I'm individually testing my service in a *dev* environment to investigate bugfixes and new features. 

:person_with_blond_hair: *Tom:* I'm a developer and responsible for building the frontend service of the app *foo*. I'm individually testing my service in a *dev* environment to investigate bugfixes and new features. 

*Tim & Tom*: For an end-2-end performance test, which is conducted in *hardening* stage, we need to deploy the two services as a couple, followed by a performance test for the couple. 

### Initial situation

*Project:* foo

*Services:*  foobar-frontend <--> foobar-backend <--> foobar-db (direct)

*Stage:*
- dev: For functionals test and bug investigation
- hardening: For performance tests of a set of microservices
- production: Is out of scope for these users (Tim & Tom). Does not matter, just needs to work.

### Domain events: Tim and Tom send a new artifact at the same time

Tim, I want to release my new version of the backend service: 

```console
keptn send new-artifact 
  --project=foo
  --service=foobar-backend
  --artifact=foobar-backend:0.22.3.rc
```

Tom, I want to release my new version of the frontend service:

```console
keptn send new-artifact 
  --project=foo
  --service=foobar-frontend
  --artifact=foobar-frontend:0.9.42 
```

Tim & Tom meet for a coffee and decide to push their latest artifacts to hardening (for their performance tests): 

```console
keptn send new-artifact --project=foo --stage=hardening
  --service=foobar-frontend
  --artifact foobar-backend:0.22.3.rc 
  --service=foobar-frontend
  --artifact=foobar-frontend:0.9.42 
```

</p>
</details>

### Desired outcome

Both, Tim and Tom, expect that their service is deployed in *dev*. Afterwards, both would like to promote their artifacts together to *hardening*. Once all end-2-end tests in hardening have passed, they should be automatically promoted to production.

## Event stream

### Dev stage:

*Event stream triggered by Tim:* 

- artifact-delivery.started
  - update.triggered 
    - update.started
    - update.finished
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
- artifact-delivery.finished 

*Event stream triggered by Tom:* 

- artifact-delivery.started
  - update.triggered 
    - update.started
    - update.finished
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
- artifact-delivery.finished 

### Hardening stage

*Event stream triggered by Tom & Tim:*

- artifact-delivery.started
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
- artifact-delivery.finished 

### Production stage: the usual workflow

---

## Internal flow - outdated

**Initial state**:

- pod1: foobar-frontend:1.2.0
- pod2: foobar-backend:1.2.0

**Sending new-artifact**:

```json
{
    "type": "new-artifact",
    "project": "foo",
    "services": [
        "foobar-frontend:1.2.3",
        "foobar-backend:1.2.3"
    ]
}
```

**Deployment service starts working**:

- pod1 [blue]: foobar-frontend:1.2.0
- pod2 [blue]: foobar-backend:1.2.0
- pod3 [green]: foobar-frontend:1.2.3
- pod4 [green]: foobar-backend:1.2.3

**Deployment finished event**:

```json
{
    "type": "deployment-finished",
    "project": "foo",
    "services": [
        "foobar-frontend:1.2.3",
        "foobar-backend:1.2.3"
    ],
    "test_uris": [
        "foobar-frontend.green.....keptn.sh",
        "foobar-backend.green....keptn.sh"
    ]
}
```

**Tests**:

During tests only green services can communicate with each other.

release:
- pod3 [green]: foobar-frontend:1.2.3
- pod4 [green]: foobar-backend:1.2.3
