# Labels

## Goal
- Labels are kee/value pairs that can be attached to objects.
  - Labels are like tags in AWS or other cloud providers, used to tag resources
- You can label your objects, for instance your pod, following an organizational structure.
  - <b>Key</b>: enviroment - <b>Value</b>: dev/staging/qa/prod
  - <b>Key</b>: department - <b>Value</b>: engineering/finance/marketing
- Labels are not unique and multiple labels can be added to on object
- Once labels are attached to an object, you can use filters to narrow down results.
  - This is called <b>Label Selectors</b>
- Using Label Selectors, you can see <b>matching expressions</b> to match labels
  - For instance, a particular pod can only run on a node labeled with `environment` equals `development`
  - More complex matching: `environment` in `development` or `qa`
- You can also use labels to tag nodes
- Once node are tagged, you can use <b>label selectors</b> to let pods only run on <b>specific nodes</b>
- There are 2 steps required to run a pod on a specific set of nodes:
  - First you tag the node
  - Then you add a `nodeSelector` to your pod configuration
- First step, add a label or multiple labels to your nodes:
```terminal
    $ kubectl label nodes node1 hardware=high-spec
    $ kubectl label nodes node2 hardware=low-spec
```
- Secondly, add a pod that uses those labels:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-first-app-pod
  labels:
    name: my-first-app
spec:
  containers:
  - name: my-first-app
    image: vuduchailinh/my-first-app
    resources:
      limits:
        memory: "128Mi"
        cpu: "500m"
    ports:
      - containerPort: 3000
  nodeSelector:
    hardware: high-spec
```