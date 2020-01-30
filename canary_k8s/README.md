# Release Brackets

As a user, I want to do a canary release on K8s

## User Facing

**Example Project Structure**: foobar-frontend <--> foobar-backend <--> foobar-db (direct)

### Releasing a new version of foobar-frontend

```console
keptn send new-artifact --project=foo --service=foobar-frontend --artifact foobar-frontend:1.2.3
```

- [Shipyard](../0_basics/shipyard.yaml)
- [Uniform](../0_basics/uniform.yaml)

