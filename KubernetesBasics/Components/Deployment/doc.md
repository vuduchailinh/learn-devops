## 1. Deployments
- A deployment declaration in Kubenetes allows you to do app <b>deployment</b> and <b>updates</b>
- When using the deployment object, you define the state of your application
  - Kubernetes will then make sure the cluster matches your <b>desired</b> state
- Just using the <b>replication controller</b> or <b>replication set</b> might be cumbersome to deploy apps
  - The <b>Deployment Object</b> is easier to use and gives you more possibilities
  
With a deployment object you can:
- Create a deployment (e.g deploying an app)
- Update a deployment (e.g deploying a new version)
- Do rolling updates (zero downtime deployments)
- Roll back to a previous version
- Pause/Resume a deployment (e.g to roll-out to only a certain percentage)

## 2. Useful commands
```terminal
    # Create deployment
    $ kubectl create -f <file name>
    # Get information on current deployments
    $ kubectl get deployments

    # Get information about the replica sets
    $ kubectl get rs

    # Get pods, and also show labels attached to those pods
    $ kubectl get pods --show-labels

    # Run k8s-demo with the image lable orther version
    $ kubectl set image deployment/<deployment name> k8s-demo=k8s-demo:2

    # Edit the deployment object
    $ kubectl edit deployment/<deployment name>

    # Get the rollout history
    $ kubectl rollout history deployment/<deployment name>

    # Rollback to previous version
    $ kubectl rollout undo deloyment/<deployment name>

    # Rollback to any version revision
    $ kubectl rollout undo deployment/<deployment name> --to-revision=n
```

## 3. Demo

Create file `deployment-myFirstApp.yml`

```terminal
  # Create new deployment
  $ kubectl create -f deployment/deployment-myFirstApp.yml

  # Get deployment info
  $ kubectl get deployments [-o wide]

  # Expose deployment (create new a service)
  $ kubectl expose deployment my-first-app-deployment --type=NodePort

  # Change version my-first-app to version 2
  $ kubectl set image deployment/my-first-app-deployment my-first-app=vuduchailinh/my-first-app:2

  # Check change
  $ kubectl rollout status deployment/my-first-app-deployment
```