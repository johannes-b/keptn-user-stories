# Hooks

*Last update of shipyard and uniform: 20.02.2020*

As a user, I want to register a webhook for a Keptn project, stage, service and task.

As a user, I want to provide a Docker image, which is used for tests/remediations​.

## User view

:man: *Arnold*: I'm a DevOps engineer and I want to use a webhook to notify an external service about the releasing task.

:blonde_woman: *Sue*: I'm an acceptance engineer and I want my tests, which are executed in a Docker image, to be executed as part of a configuration-change in the hardening stage.

### Initial situation

No specific setup, normal project setup (e.g., carts and carts-db) with a normal [shipyard.yaml](../0_basics/shipyard.yaml) and [uniform.yaml](../0_basics/shipyard.yaml).

### Domain events

**1.) Arnold adds a webhook for release**

```console
keptn register hook --project=sockshop --service=carts [--stage=*] 
   --task=release https://some-host.com/hook/me/up/
```

**2.) Sue adds a docker image for tests in hardening stage**

```console
keptn register hook --project=sockshop --service=carts --stage=hardening 
   --task=test docker.io/myusername/my-test-image:1.5.77
```

**3.) At some later point: Anyone triggers a artifact-delivery**

I want to release my new version of the carts service: 
```console
keptn trigger configuration-change --project=sockshop
  --service=carts
  --artifact=carts:0.10.3
```

### Desired outcome

Arnold wants the webhook to receive the `test` event including the entire payload of the event. If the webhook responds with http status code `2xx` the tests have successfully finished, else they have failed.

Sue wants her tests to be executed, e.g., `docker run docker.io/myusername/my-test-image:1.5.77`. If the container exits with `0` the tests have successfully finished, else they have failed.

## Event stream

### Dev stage:

*Event stream triggered by any Dev:* 

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
      - **Webhook fired** https://some-host.com/hook/me/up/ - responds with HTTP Status 200
    - release.finished
- artifact-delivery.finished 

### Hardening stage

*Event stream continued due to pull artifact-delivery from dev* 

- artifact-delivery.started
  - update.triggered 
    - update.started
    - update.finished
  - deployment.triggered 
    - deployment.started 
    - deployment.finished
  - test.triggered
    - test.started
      - **Container triggered** ``docker run docker.io/myusername/my-test-image:1.5.77``, waits for it to exit (e.g., max 30 minutes)
    - test.finished
  - evaluation.triggered
    - evaluation.started
    - evaluation.finished
  - release.triggered
    - release.started
      - **Webhook fired** https://some-host.com/hook/me/up/ - responds with HTTP Status 200
    - release.finished
- artifact-delivery.finished  

### Production stage

The usual workflow.