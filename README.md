# KUBERNETES (k8s-resources)

## 1. Minikube basic commands

- minikube start
- minikube stop
- minikube status
- minikube delete

## 2. Kubectl basic commands

Generic command for check short-names: `kubectl api-resources`

### 2.1. Pods

| Description                                        | Command                                                                          |
| :------------------------------------------------- | :------------------------------------------------------------------------------- |
| 1. List the pods                                   | `kubectl get pods`                                                               |
| 2. To create a pod                                 | `kubectl run --generator=run-pod/v1 podtest --image=nginx:alpine`                |
| 3. More pod information                            | `kubectl describe pod <pod-name>`                                                |
| 4. To see k8s resources                            | `kubectl api-resources`                                                          |
| 5. To delete a pod                                 | `kubectl delete pod <pod-name>`                                                  |
| 6. To filter specific pod                          | `kubectl get pod <pod-name>`                                                     |
| 7. To see pod manifesto                            | `kubectl get pod <pod-name> -o yaml`                                             |
| 8. To forwarding pod port                          | `kubectl port-forward podtest 7000:80`                                           |
| 9. To go into a pod bash                           | `kubectl exec -ti <pod-name> -- sh`                                              |
| 10. To see pod logs                                | `kubectl logs <pod-name>`                                                        |
| 11. To see API version                             | `kubectl api-versions`                                                           |
| 12. To see the kind of element                     | `kubectl api-resources | grep Pod`                                               |
| 13. To see container logs in a 2 contianer pod     | `kubectl logs <pod-name> -c <cont + contNumber>`                                 |
| 14. Sh into a container in a pod with 2 containers | `kubectl exec -ti <pod-name> -c cont1 -- sh`                                     |
| 15. Filter pod by label                            | `kubectl get pods -l <metadata-label-key>=<metadata-label-value`>                |
| 16. Owner reference (applied by ReplicaSet)        | `kubectl get pod <pod name> -o yaml`                                             |
| 17. To output more pod information (Ips)           | `kubectl get pods -l <label-key>=<label-value> -o wide`                          |
| 18. 18. To create a temporal pod                   | `kubectl run --rm -ti --generator=run-pod/v1 podtest --image=nginx:alpine -- sh` |

### 2.2. ReplicaSet

| Description                                            | Command                                       |
| :----------------------------------------------------- | :-------------------------------------------- |
| 1. List replicaSets                                    | `kubectl get rs`                              |
| 2. Describe a replicaSet                               | `kubectl describe rs <replicaSet name>`       |
| 3. Describe in YAML manifesto (check rs configuration) | `kubectl get rs <replicaSet name> -o yaml`    |
| 4. Filter replicaset by label                          | `kubectl get rs -l <label-key>=<label-value>` |

### 2.3. Deployment

| Description                             | Command                                                                             |
| :-------------------------------------- | :---------------------------------------------------------------------------------- |
| 1. List deployments                     | `kubectl get deployment`                                                            |
| 2. To see deployment labels             | `kubectl get deployment --show-labels`                                              |
| 3. To see deployment status             | `kubectl rollout status deployment <deployment-name>`                               |
| 4. To see deployment description        | `kubectl describe deployment <deployment-name>`                                     |
| 5. To see deployment yaml description   | `kubectl get deployment <deployment-name> -o yaml`                                  |
| 6. To update the deployment             | `kubectl apply -f dep.yaml`                                                         |
| 7. To see the deployment update process | `kubectl rollout status deployment <deployment-name>`                               |
| 8. To describe deployments              | `kubectl describe deployment <deployment-name>`                                     |
| 9. To see deployment history            | `kubectl rollout history deployment <deployment-name>`                              |
| 10. To see an specific history REVISION | `kubectl rollout history deployment <deployment-name> --revision=<number-revision>` |
| 11.  To rollback a deployment           | `kubectl rollout undo deployment <deployment-name> --to-revision=<revision-number>` |

### 2.4 Services

| Description                | Command                                        |
| :------------------------- | :--------------------------------------------- |
| 1. List services           | `kubectl get svc`                              |
| 2. To filter the services  | `kubectl get svc -l <label-key>=<label-value>` |
| 3. To describe the service | `kubectl describe svc <service-name>`          |
| 4. To get the endpoints    | `kubectl get endpoints`                        |

### 2.5 Namespaces

| Description                         | Command                                                                               |
| :---------------------------------- | :------------------------------------------------------------------------------------ |
| 1. List namespaces                  | `kubectl get namespaces`                                                              |
| 2. To see pod inside a namespace    | `kubectl get pods --namespace <namespace-name>`                                       |
| 3. To get all inside a namespace    | `kubectl get all -n <namespace-name>`                                                 |
| 4. To create a namespace            | `kubectl create namespace <namespace-name>`                                           |
| 5. To see namespaces labels         | `kubectl get namespaces --show-labels`                                                |
| 6. To describe a namespace          | `kubectl describe namespaces <namespace-name>`                                        |
| 6. To run a pode inside a namespace | `kubectl run --generator=run-pod/v1 podtest --image=nginx:alpine -n <namespace-name>` |
| 7. To delete a namespace            | `kubecttl delete namespaces <namespace-name>`                                         |

### 2.6 Context

| Description                     | Command                                                                                              |
| :------------------------------ | :--------------------------------------------------------------------------------------------------- |
| 1. To get current context       | `kubectl config current-context`                                                                     |
| 2. To see current config        | `kubectl config view`                                                                                |
| 3. To create a context          | `kubectl config set-context <context-name> --namespace=<ns-name> --cluster=minikube --user=minikube` |
| 4. To switch to another context | `kubectl config use-context <context-name>`                                                          |

### 2.7 POD limits RAM and CPU

| Description                   | Command                             |
| :---------------------------- | :---------------------------------- |
| 1. To see the use of the NODE | `kubectl get nodes`                 |
| 2. Describe the NODE          | `kubectl describe node <node-name>` |

### 2.8 YAML file

| Description                                         | Command                                                |
| :-------------------------------------------------- | :----------------------------------------------------- |
| 1. To apply a yaml manifesto (-f = file)            | `kubectl apply -f pod.yaml`                            |
| 2. To delete all resources running from a yaml file | `kubectl delete -f pod.yaml`                           |
| 3. To see command run in the history (CHANGE-CAUSE) | `kubectl rollout history deployment <deployment-name>` |

## 3. YAML Notes

### 3.1. Pods

#### 3.1.1 Labels (Very important)

Metadata to indicate pod metadata in case we have several equal pods but for
different purposes (different app, environment...). Be careful the pod name is
not inside the labels medatadata. Also used by replicaset and deployment for
managing the pods.

#### 3.1.2. Owner references

Indicates the parent owner of the replicaSet.

#### 3.1.3. Image policy

To check for images locally before looking to remote images:

```yaml
  spec:
    containers:
    - name: backend
      image: k8s-hands-on
      imagePullPolicy: IfNotPresent
```

### 3.2. Replicaset

- apiVersion: apps/v1. apps = to understand the prefix run `kubectl api-resources`
and look for NAME = replicaset and you will find the the its GROUP is apps.
- kind: to find the KIND field just do the same as the previous step and find the
KIND field.
- ownerReferences: indicates the parent owner of the replicaSet.

### 3.3. Deployment

By default it has .spec.revisionHistoryLimit = 10 (ReplicaSet history limit)

#### 3.3.1 CHANGE-CAUSE

It is the cause of the deployment.</br>
To see the deployment CHANGE-CAUSE: `kubectl rollout history deployment <deployment-name>`. </br>
To save command the in the history (CHANGE-CAUSE):

  1. `kubectl rollout history deployment <deployment-name>`.
  2. Add to YAML, after metadata:

        ```yaml
        annotations:
            kubernetes.io/change-cause: "Changes port to 110"
        ```

#### 3.3.2 Rollback

The idea is that, for example a pod is not able to start due to a issue, you can
rollback to a previous version by executing:

```bash
kubectl rollout undo deployment <deployment-name> --to-revision=<revision-number>
```

Remember that by default we have 10 old ReplicaSet old version to rollback to,
in case it is necessary.

### 3.4 Services

By default TYPE = ClusterIp. It is a virtual Ip that works as the entrance of our pods.
name: *my-service* :arrow_right: It is also the DNS of the service.

#### 3.4.1. ClusterIp

Exposes the Service on a cluster-internal IP. Choosing this value makes the
Service only reachable from within the cluster. This is the default `ServiceType`.

```yaml
[...]
  spec:
    type: ClusterIp
```

#### 3.4.2. NodePort

 Exposes the Service on each Node's IP at a static port (the NodePort). A
 ClusterIP Service, to which the NodePort Service routes, is automatically
 created. You'll be able to contact the NodePort Service, from outside the
 cluster, by requesting `<NodeIP>`:`<NodePort>`.

```yaml
[...]
  spec:
    type: NodePort
```

For checking the NodePort execute:
`kubectl get svc -l <label-key>=<label-value>`
And check the PORTs field and see the mapped port: `8080:32358/TCP`.