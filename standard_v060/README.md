# Standard Keptn 0.6.0 User Story

This use story shows the standard flow as implemented in Keptn 0.6.0.

## User view

-

### Initial situation

No specific setup, normal project setup (e.g., carts and carts-db) with a normal [shipyard.yaml](../0_basics/shipyard.yaml) and [uniform.yaml](../0_basics/shipyard.yaml).

### Domain events

**Arnold adds a webhook for promote**

```console
keptn register hook --project=sockshop --service=carts [--stage=*] 
   --action=promote https://some-host.com/hook/me/up/
```

**Sue adds a docker image for tests in hardening stage**

```console
keptn register hook --project=sockshop --service=carts --stage=hardening 
   --action=test docker.io/myusername/my-test-image:1.5.77
```

**At some later point: Anyone triggers a configuration-change**

I want to release my new version of the carts service: 
```console
keptn trigger configuration-change --project=sockshop
  --service=carts
  --artifact carts:0.10.3
```

### Desired outcome

Arnold wants the webhook to receive the `test` action/event (including payload of the event). If the webhook responds with http status code `2xx` the tests have successfully finished, else they have failed.

Sue wants her tests to be executed, e.g., `docker run docker.io/myusername/my-test-image:1.5.77`. If the container exits with `0` the tests have successfully finished, else they have failed.

## Event stream

### Dev stage:

*Event stream triggered by any Dev:* 
- configuration-change.triggered
  - deploy_direct.triggered # shipyard controller
    - deploy_direct.started # Keptn service
    - deploy_direct.finished
  - functional_tests.triggered
    - functional_tests.started
    - functional_tests.finished
  - evaluation.triggered
    - evaluation.started
    - evaluation.finished
  - release.triggered
    - release.started
    - release.finished
  - promote.triggered
    - promote.started
      - **Webhook fired** https://some-host.com/hook/me/up/ - responds with HTTP Status 200
    - promote.finished
- configuration-change.finished

### Hardening stage

*Event stream continued due to promote action in dev:* 
- configuration-change.triggered
  - deploy_blue_green.triggered 
    - deploy_blue_green.started 
    - deploy_blue_green.finished
  - functional_tests.triggered
    - functional_tests.started
      - **Container triggered** ``docker run docker.io/myusername/my-test-image:1.5.77``, waits for it to exit (e.g., max 30 minutes)
    - functional_tests.finished
  - performance_tests.triggered
    - performance_tests.started
    - performance_tests.finished
  - evaluation.triggered
    - evaluation.started
    - evaluation.finished
  - release.triggered
    - release.started
    - release.finished
  - promote.triggered
    - promote.started
      - **Webhook fired** https://some-host.com/hook/me/up/ - responds with HTTP Status 200
    - promote.finished
- configuration-change.finished


### Production stage

the usual workflow (there is no `promote` action available).