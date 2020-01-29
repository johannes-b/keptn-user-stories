# Release Brackets

As a user, I want to queue services in staging and finally release them together (even in a canary-way)



## User Facing

**Example Project Structure**: foobar-frontend <--> foobar-backend <--> foobar-db (direct)

### Release both at the same time
As a user, instead of releasing artifacts one per one, e.g.,
```
keptn send new-artifact --project=foo --service=foobar-frontend
   --artifact foobar-frontend:1.2.3
keptn send new-artifact --project=foo --service=foobar-backend
   --artifact foobar-backend:1.2.3
```

I want to release multiple artifacts together, e.g.,
```
keptn send new-artifact --project=foo 
   --service=foobar-frontend --artifact foobar-frontend:1.2.3 
   --service=foobar-backend --artifact foobar-backend:1.2.3
```

## Internals

**Before**:

- pod1: foobar-frontend:1.2.0
- pod2: foobar-backend:1.2.0


**New Artifact is sent**:

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

**Deployment service starts working**

during:
- pod1 [blue]: foobar-frontend:1.2.0
- pod2 [blue]: foobar-backend:1.2.0
- pod3 [green]: foobar-frontend:1.2.3
- pod4 [green]: foobar-backend:1.2.3

**Deployment Finished event**:

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
