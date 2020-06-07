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
| 13. To apply a yamk manifesto                        | `kubectl apply -f pod.yaml`                                       |
| 14. To delete all resources running from a yaml file | `kubectl delete -f pod.yaml`                                      |