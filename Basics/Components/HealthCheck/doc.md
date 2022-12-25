# Health Checks

## Goal
- If your application `malfunctions`, the pod and container can still be running, but the application might not work anymore
- To `detect` and `resolve` problems with your application, you can run `health checks`
- You can run 2 difference type of health checks
  - Running a `command` in the container periodically
  - Periodic checks on a `URL` (HTTP)
- The typical production application behind a load balancer should always have `health checks` implemented in some way to ensure `availability` and `resiliency` of the app.

### Readiness Probe:
- Besides livenessProbes, you can also use `readnessProbes` on a container with a Pod
- `livenessProbes`: indicates whether a container is `running`
  - If the check fails, the container with be restarted
- `readinessProbes`: indicates whether the container is `ready to serve` requests
  - If the check fails, the container `with not be restarted`, but the `Pod's IP address with be removed from the Service`, so it'll not serve any requests anymore
- The `readinessProbes` test will make sure that `at startup`, the pod will only receive traffic when the test succeeds
- You can use these probes in `conjunction`, and you can configure different tests for them
- If your container always exits when somthing goes wrong, you don't need a livenessProbe
- In general, you configure `both` the livenessProbe and the readinessProbe

For example:
```yaml
livenessProbe:
    httpGet:
        path: /
        port: nodejs-port
    initialDelaySeconds: 15
    timeoutSeconds: 30
readinessProbe:
    httpGet:
        path: /
        port: nodejs-port
    initialDelaySeconds: 15
    timeoutSeconds: 30
```