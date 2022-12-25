# Secrets

## Goal
- Secrets provides a way in Kubernetes to distribute `credentials, keys, passwords` or `secret data` to the pods
- Kubernetes itself uses this Secrets mechanism to provide the credentials to access the internal API
- You can also use the `same mechanism` to provide secrets to your application
- Secrets is one way to provide secrets, native to Kubernetes
  - There are still `other way` your container can get its secrets if you don't want to use Secrets (e.g. using an `external vault service` in your app)
  
- Secrets can be used in the following ways:
  - Use secrets as `environment variables`
  - Use secrets as a file in a pod
    - This setup uses `volumes` to be mounted in a container
    - In this volume you have files
    - Can be used for instance for `dotenv` files or your app can just read this file
  - Use an `external image` to pull secrets (from a `private image registry`)

- To generate secrets using files:
```terminal
    $ echo -n "root" > ./username.txt
    $ echo -n "password" > ./password.txt
    $ kubectl create secret generic db-user-pass --from-file=./username.txt --from-file=./password.txt
    # Result: secret "db-user-pass" created
```
- A secret can also be an SSH key or an SSL certificate
```terminal
    $ kubectl create secret generic ssl-certificaate --from-file=ssh-privatekey=~/.ssh/id_rsa --ssl-cert-=ssl-cert=mysslcert.crt
```
## To generate secrets using yaml definitions:
Create file `secrets-db-secret.yaml`
```yml
    apiVersion: v1
    kind: Secret
    metadata:
        name: db-secret
    type: Opaque
    data:
        password: cm9vdA==
        username: cGFzc3dvcmQ=
```

Generate the username and password
```terminal
    $ echo -n "root" | base64
    $ echo -n "password" | base64
```

Create secret
```terminal
    $ kubectl create -f secrets-db-secret.yaml
    # Result: secret "db-secret" created
```

## Using secrets:
- You can create a pod that exposes the secrets as environment variables
```yaml
    kind: Pod
    ...
    spec:
        containers:
            - name: secret-demo
            ...
            env:
            - name: SECRET_USERNAME
              valueFrom:
                secretKeyRef:
                    name: db-secret
                    key: username
            - name: SECRET_PASSWORD
              valueFrom:
                secretKeyRef:
                    name: db-secret
                    key: password
                
```
- Alternatively, you can provide the secrets in a file
```yaml
    kind: Pod
    ...
    spec:
        containers:
            - name: secret-demo
            ...
            volumeMounts:
            - name: credvolume
              # The secrets will be stored in:
              # - /etc/creds/db-secrets/username
              # - /etc/creds/db-secrets/password
              mountPath: /etc/creds 
              readOnly: true
        volumes:
        - name: credvolume
          secret:
            secretName: db-secrets
```

## Demo
```terminal
    # Create secret file `secrets-myFirstApp.yml`
    $ kubectl create -f secrets-myFirstApp.yml

    # Create deployment file `secrets-volumes-myFirstApp.yml`
    $ kubectl create -f secrets-volumes-myFirstApp.yml
    # Describe one of pods will see volumes mounted
```