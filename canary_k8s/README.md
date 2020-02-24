# Canary

*Last update of shipyard and uniform: 24.02.2020*

## User view


### Initial situation


### Domain events: Problem occured in Production


### Desired outcome


## Event stream

### Production stage:

*Event stream triggered by: hardening.artifact-delivery.finished:* 

- deployment.started
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
- deployment.finished

- release.started
  - approval.triggered
    - approval.started
    - approval.finished
  - traffic_shift.triggered
    - traffic_shift.started    
    - traffic_shift.in-progress    # set 50% of traffic to canary
- release.in-progress

- release_validation.started
  - test.triggered
    - test.started
    - test.finished
  - evaluation.triggered
    - evaluation.started
    - evaluation.finished
- release_validation.finished

- release.started
  - approval.triggered
    - approval.started
    - approval.finished
  - traffic_shift.triggered
    - traffic_shift.started    
    - traffic_shift.in-progress    # set 100% of traffic to canary
- release.in-progress

- release_validation.started
  - test.triggered
    - test.started
    - test.finished
  - evaluation.triggered
    - evaluation.started
    - evaluation.finished
- release_validation.finished

- release.started
  - approval.triggered
    - approval.started
    - approval.finished
  - traffic_shift.triggered
    - traffic_shift.started    
    - traffic_shift.finished   # done
- release.finished
