# KUBERNETES (k8s-resources)

## Minikube basic commands

- minikube start
- minikube stop
- minikube status
- minikube delete

## Kubectl basic commands

Generic command for check short-names: `kubectl api-resources`

### Pods

| Description                                        | Command                                                           |
| :------------------------------------------------- | :---------------------------------------------------------------- |
| 1. List the pods                                   | `kubectl get pods`                                                |
| 2. To create a pod                                 | `kubectl run --generator=run-pod/v1 podtest --image=nginx:alpine` |
| 3. More pod information                            | `kubectl describe pod <pod-name>`                                 |
| 4. To see k8s resources                            | `kubectl api-resources`                                           |
| 5. To delete a pod                                 | `kubectl delete pod <pod-name>`                                   |
| 6. To filter specific pod                          | `kubectl get pod <pod-name>`                                      |
| 7. To see pod manifesto                            | `kubectl get pod <pod-name> -o yaml`                              |
| 8. To forwarding pod port                          | `kubectl port-forward podtest 7000:80`                            |
| 9. To go into a pod bash                           | `kubectl exec -ti <pod-name> -- sh`                               |
| 10. To see pod logs                                | `kubectl logs <pod-name>`                                         |
| 11. To see API version                             | `kubectl api-versions`                                            |
| 12. To see the kind of element                     | `kubectl api-resources | grep Pod`                                |
| 13. To see container logs in a 2 contianer pod     | `kubectl logs <pod-name> -c <cont + contNumber>`                  |
| 14. Sh into a container in a pod with 2 containers | `kubectl exec -ti <pod-name> -c cont1 -- sh`                      |
| 15. Filter pod by label                            | `kubectl get pods -l <metadata-label-key>=<metadata-label-value`> |
| 16. Owner reference (applied by ReplicaSet)        | `kubectl get pod <pod name> -o yaml`                              |

### ReplicaSet

| Description                                            | Command                                    |
| :----------------------------------------------------- | :----------------------------------------- |
| 1. List replicaSets                                    | `kubectl get rs`                           |
| 2. Describe a replicaSet                               | `kubectl describe rs <replicaSet name>`    |
| 3. Describe in YAML manifesto (check rs configuration) | `kubectl get rs <replicaSet name> -o yaml` |

### YAML file

| Description                                         | Command                      |
| :-------------------------------------------------- | :--------------------------- |
| 1. To apply a yaml manifesto (-f = file)            | `kubectl apply -f pod.yaml`  |
| 2. To delete all resources running from a yaml file | `kubectl delete -f pod.yaml` |

## YAML Notes

### Pods

#### Labels (Very important)

Metadata to indicate pod metadata in case we have several equal pods but for
different purposes (different app, environment...). Be careful the pod name is
not inside the labels medatadata. Also used by replicaset and deployment for
managing the pods.

#### Owner references

Indicates the parent owner of the replicaSet.

### Replicaset

- apiVersion: apps/v1. apps = to understand the prefix run `kubectl api-resources`
and look for NAME = replicaset and you will find the the its GROUP is apps.
- kind: to find the KIND field just do the same as the previous step and find the
KIND field.
- ownerReferences: indicates the parent owner of the replicaSet.
