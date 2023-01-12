# Service discovery

## DNS
- As of kubernetes 1.3, DNS is a `built-in` service launched automatically using the addon manager
  - The addons are in the /etc/kubernetes/addons `directory` on `master node`
- The DNS service can be used within pods to `find other service` running on the same cluster
- Multiple containers `within 1 pod` don't need this service, as they can `contact` each other `directly`
  - A container in the same pod can connect to the port of the other container directly using `localhost:port`
- To make DNS work, a pod will need a `Service definition`

## Demo
Create file `secrets.yml, database-pod.yml, database-service.yml`

```bash
  # create secret
  $ kubectl create -f secrets.yml
```