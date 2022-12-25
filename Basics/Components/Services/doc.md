# Services

## Goal
-   Pods are very dynamic, they come and go on the Kubernetes cluster
    -   When using a Replication Controller, pods are terminated and created during scaling operations.
    -   When using Deployments, when updating the image verion, pods are terminated and new pods take the place of older pods.

- That's why Pods should never be accessed directly, but always through a Service
- A service is the logical bridge between the 'mortal' pods and other services or end-users.
- When using the `kubetctl expose` command earlier, you created a new Service for your pod, so it could be accessed externally.
- Create a service will create an endpoint for your pods:
  - a <b>ClusterIP</b>: a virtual IP address only reachable from within the cluster (this is the default)
  - a <b>NodePort</b>: a port that is the same on each node that is alse reachable externally
  - a <b>LoadBalancer</b>: a LoadBalancer created by the <b>cloud provider</b> that will route external traffic to every node on the NodePort (ELB on AWS)
- The options just shown only allow you to create <b>virtual IPS or ports</b>
- There is also a possibility to use <b>DNS names</b>
  - <b>ExternalName</b> can provide a DNS name for the service (e.g. for service discovery using DNS)
  - This only works when the <b>DNS add-on</b> is enabled

## Demo
Create file `service-myFirstApp.yml`

```terminal
    # create new pod
    $ kubectl create -f ../../MyFirstApp/pod-myFirstApp.yml

    # create new service
    $ kubectl create -f service-myFirstApp.yml

    # get service info
    $ kubectl describe my-first-app-service

    # delete a service
    $ kubectl delete service my-first-app-service
```