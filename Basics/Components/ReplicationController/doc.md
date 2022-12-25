## 1. Replication Controller
- Scaling in kubernetes can be done using the <b>Replication Controller</b>
- The replication controller with ensure a specified number of pod replicas will run at all time
- A pods created with the replica controller will automatically be replaced if they fail, get deleted, or are teminated.
- Using the replication controller is also <b>recommended</b> if you just want to make sure 1 pod is always running, even after reboots
  - You can then run a replication controller with just 1 replica
  - This makes sure that the pod is always running

### Demo
Create file `replicationController-myFirstApp.yml`

1. Create controller
```terminal
    $ cd Components
    $ kubectl create -f <replication controller yml file>

    # Get replication controller
    $ kubectl get rc

    # If pod crashes, then the controller with autoatically create new pod
    # Remove a pod
    $ kubectl delete pod <pod name>

    # New pod will created
    $ kubectl get pods

    # Scale pods using kubectl command
    $ kubectl scale --replicas=4 -f <replication controller yml file>

    # Delete replication controller
    $ kubectl delete rc/<replication controller name>
```