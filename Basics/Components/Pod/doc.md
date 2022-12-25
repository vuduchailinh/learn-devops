# Pod

## Pod state
- Pods have a status field, which you see when you do `kubectl get pods`
```terminal
NAME                              READY   STATUS    RESTARTS   AGE
my-first-app.vuduchailinh.store   1/1     Running   0          23h
```
- In this scenario all pod in the `running`
  - This means that the `pod has been bound to a node`
  - All `containers have bean created`
  - `At least one container` is still `running`, or is starting/restarting
- Other valid statuses are:
  - `Pending`: Pod has been `accepted` but is `not running`
    - Happens when the container image is still `downloading`
    - If the pod cannot be scheduled because of `resource containers`, it'll also be in this status (don't have enough CPU or memory)
  - `Succeeded`: All containers within this pod have been `terminated successfully` and will not be restarted.
  - `Failed`: All containers with this pod have been `Terminated`, and at least one container retured a failure code.
    - The failure code is the `exit code` of the process when a container terminates.
  - `Unknown`: The `state of the pod couldn't be determined
    - A `network error` might have been occurred (for example the node where the pod is running on is down)
  - You can get the `pod condition` using kubectl get pod `<POD NAME>`
    ```terminal
        $ kubectl get pod my-first-app-pod

        ## result
        Conditions:
            Type              Status
            Initialized       True 
            Ready             True 
            ContainersReady   True 
            PodScheduled      True 
    ```
    - These are conditions which the pod has passed
      - In this example, Initialized, Ready, ContainersReady, PodScheduled

There are 5 different types of `PodCondition`:
- `PodScheduled`: the pod has been scheduled to a node
- `Ready`: Pod can serve requests and is going to be added to matching Services
- `Initialized`: the initialization containers have been stated successfully
- `Unschedulable`: the Pod can't be scheduled (for example due to resource constraints)
- `ContainersReady`: all containers in the pod are ready