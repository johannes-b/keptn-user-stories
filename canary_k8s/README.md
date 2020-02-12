# Canary

*Last update of shipyard and uniform: 31.01.2020*

## User view


### Initial situation


### Domain events: Problem occured in Production


### Desired outcome


## Event stream

### Production stage:

*Event stream triggered by Configuration change for production:* 
- configuration-change.triggered
  - deploy_blue_green.triggered 
    - deploy_blue_green.started 
    - deploy_blue_green.finished
  - functional_tests.triggered
    - functional_tests.started
    - functional_tests.finished
  - evaluation.triggered
    - evaluation.started  
    - evaluation.finished
  - manual_approval.triggered
    - manual_approval.started
    - manual_approval.finished
  - release_canary.triggered
    - release_canary.started    
    - release_canary.progressed # set 10% of traffic to canary
  - real_user_tests.triggered
    - real_user_tests.started
    - real_user_tests.finished
  - evaluate_release.triggered
    - evaluate_release.started
    - evaluate_release.finished
  - release_canary.retriggerd
    - release_canary.progressed # set 20% of traffic to canary
  - real_user_tests.triggered
    - real_user_tests.started
    - real_user_tests.finished
  - evaluate_release.triggered
    - evaluate_release.started
    - evaluate_release.finished 
  - release_canary.retriggerd
    - release_canary.finished
- configuration-change.done

---

