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
- production: Alice is responsible for this stage and has therefore defined remedation actions. 

*Shipyard:* (compared to default)
- ...

*Remedation actions*


### Domain events: Problem occured in Production

*Domain event source:* Monitoring solution (e.g., Dynatrace)



### Desired outcome


## Event stream

### Production stage:

*Event stream triggered by Domain event source (Dynatrace):* 
- problem-open.triggered
  - remedation.triggered 
    - remedation.started 
    - remedation.progressed     # Execute 1st action
  - real_user_tests.triggered
    - real_user_tests.started
    - real_user_tests.finished
  - evaluate_remedation.triggered
    - evaluate_remedation.started  
    - evaluate_release.finished
  - remedation.retriggered
    - remedation.progressed     # Execute 2nd action
  - real_user_tests.triggered
    - real_user_tests.started
    - real_user_tests.finished
  - evaluate_remedation.triggered
    - evaluate_remedation.started  
    - evaluate_release.finished
  - remedation.retriggered
    - remedation.finished
  - escalate.triggerred
    - escalate.started
    - escalate.finished
- problem-open.finished

---

