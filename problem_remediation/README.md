# Problem Remediation

As a user, I want to define a custom remediation workflow with multiple actions.

## User view

*Alice:* I'm ???, and responsible for keeping our webshop in production alive. Alive means that our customers never face a down-time of the webshop and the responsible has to stay below 200ms since a increase would effect the convertion rate.  

### Initial situation

*Project:* webshop

*Services:*  webshop-fronted <--> payment-service, catalogue-service, user-service <--> catalogue-db, user-db

*Stage:*
- dev: Is out of scope for this user (Alice). 
- hardening: Is out of scope for this user (Alice). 
- production: Alic is responsible for production

*Shipyard:* (compared to default)
- ...

### Domain events: 


</p>
</details>

### Desired outcome

Both, Tim and Tom, expect that their service is deployed in *dev*. Afterwards, both would like to promote their artifacts together to *hardening*. Once all end-2-end tests in hardening have passed, they should be automatically promoted to production.

## Event stream

### Production stage:

*Event stream triggered by Tom:* 
- configuration-change.triggered
  - deploy_direct.triggered 
    - deploy_direct.started 
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
- configuration-change.finished

### Hardening stage

*Event stream triggered by Tom & Tim:*
- configuration-change.triggered
  - deploy_blue_green.triggered 
    - deploy_blue_green.started 
    - deploy_blue_green.finished
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
    - promote.finished
- configuration-change.finished

---

