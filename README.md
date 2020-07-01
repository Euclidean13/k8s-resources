KUBERNETES (k8s-resources)
================================================================================

Minikube basic commands
--------------------------------------------------------------------------------

- minikube start
- minikube stop
- minikube status
- minikube delete

Kubectl basic commands
--------------------------------------------------------------------------------

| Description                                          | Command                                                           |
| :--------------------------------------------------- | :---------------------------------------------------------------- |
| 1. List the pods                                     | `kubectl get pods`                                                |
| 2. To create a pod                                   | `kubectl run --generator=run-pod/v1 podtest --image=nginx:alpine` |
| 3. More pod information                              | `kubectl describe pod <pod-name>`                                 |
| 4. To see k8s resources                              | `kubectl api-resources`                                           |
| 5. To delete a pod                                   | `kubectl delete pod <pod-name>`                                   |
| 6. To filter specific pod                            | `kubectl get pod <pod-name>`                                      |
| 7. To see pod manifesto                              | `kubectl get pod <pod-name> -o yaml`                              |
| 8. To forwarding pod port                            | `kubectl port-forward podtest 7000:80`                            |
| 9. To go into a pod bash                             | `kubectl exec -ti <pod-name> -- sh`                               |
| 10. To see pod logs                                  | `kubectl logs <pod-name>`                                         |
| 11. To see API version                               | `kubectl api-versions`                                            |
| 12. To see the kind of element                       | `kubectl api-resources | grep Pod`                                |
| 13. To apply a yaml manifesto (-f = file)            | `kubectl apply -f pod.yaml`                                       |
| 14. To delete all resources running from a yaml file | `kubectl delete -f pod.yaml`                                      |
| 15. To see container logs in a 2 contianer pod       | `kubectl logs <pod-name> -c <cont + contNumber>`                  |
| 16. Sh into a container in a pod with 2 containers   | `kubectl exec -ti <pod-name> -c cont1 -- sh`                      |
| 17. Filter pod by label                              | `kubectl get pods -l <metadata-label-key>=<metadata-label-value`> |


YAML Notes
--------------------------------------------------------------------------------

### Labels (Very important)

Metadata to indicate pod metadata in case we have several equal pods but for
different purposes (different app, environment...). Be careful the pod name is
not inside the labels medatadata. Also used by replicaset and deployment for
managing the pods.
