# Dockerizing for MyFirstApp

Create files: `Dockerfile, package.json, index.js`

## 1. Build docker
```terminal
    $ cd MyFirstApp
    $ docker build -t vuduchailinh/my-first-app .
```
## 2. Run app
```terminal
    $ docker images # -> get my-first-app image id (imageId)
    $ docker run -p 3000:3000 -t <imageId>
```
## 3. Test
```terminal
    $ curl localhost:3000
```
## 4. Push an image to docker hub
```terminal
    $ docker push vuduchailinh/my-first-app
```

# Run MyFirstApp on Kubernetes

Create files `pod-myFirstApp.yml`

## 1. Use kubectl to create the pod on the kubernetes
```terminal
    $ kubectl create -f pod-myFirstApp.yml
```

## 2. Useful commands for pods
```terminal
    # Get information about all running pods
    $ kubectl get pod

    # Describe on pod
    $ kubectl describe pod <pod>

    # Expose the port of a pod (creates a new service)
    $ kubectl expose pod <pod> --type=NodePort --name=frontend

    # Port forward the exposed pod port to your local machine
    $ kubectl port-forward <pod> 8080:3000

    # Attach to the pod
    $ kubectl attach <podname> -i

    # Execute a command on the pod
    $ kubectl exec <pod> -- command

    # Add a new label to a pod
    $ kubectl label pods <pod> mylabel=myValue

    # Run a shell in a pod, useful for debugging
    $ kubectl run -i --tty busybox --image=busybox --restart=Never -- sh
```