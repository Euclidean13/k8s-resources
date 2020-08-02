# 1. KUBERNETES (k8s-resources)

<!-- omit in toc -->
- [1. KUBERNETES (k8s-resources)](#1-kubernetes-k8s-resources)
  - [1.1. Minikube basic commands](#11-minikube-basic-commands)
  - [1.2. Kubectl basic commands](#12-kubectl-basic-commands)
    - [1.2.1. Pods](#121-pods)
    - [1.2.2. ReplicaSet](#122-replicaset)
    - [1.2.3. Deployment](#123-deployment)
    - [1.2.4. Services](#124-services)
    - [1.2.5. Namespaces](#125-namespaces)
    - [1.2.6. Context](#126-context)
    - [1.2.7. POD limits RAM and CPU](#127-pod-limits-ram-and-cpu)
    - [1.2.8. LimitRange](#128-limitrange)
    - [1.2.9. ResourceQuota](#129-resourcequota)
    - [1.2.10. ConfigMaps](#1210-configmaps)
    - [1.2.11. Secrets](#1211-secrets)
    - [1.2.12. PV and PVC](#1212-pv-and-pvc)
    - [1.2.13. RBAC (Role Base Access Control): Users and Groups](#1213-rbac-role-base-access-control-users-and-groups)
    - [1.2.14. RBAC (Role Base Access Control): Service Accounts](#1214-rbac-role-base-access-control-service-accounts)
    - [1.2.15. YAML file](#1215-yaml-file)
    - [1.2.16. Generics](#1216-generics)
  - [1.3. YAML Notes](#13-yaml-notes)
    - [1.3.1. Pods](#131-pods)
      - [1.3.1.1. Labels (Very important)](#1311-labels-very-important)
      - [1.3.1.2. Owner references](#1312-owner-references)
      - [1.3.1.3. Image policy](#1313-image-policy)
    - [1.3.2. Replicaset](#132-replicaset)
    - [1.3.3. Deployment](#133-deployment)
      - [1.3.3.1. CHANGE-CAUSE](#1331-change-cause)
      - [1.3.3.2. Rollback](#1332-rollback)
    - [1.3.4. Services](#134-services)
      - [1.3.4.1. ClusterIp](#1341-clusterip)
      - [1.3.4.2. NodePort](#1342-nodeport)

## 1.1. Minikube basic commands

- minikube start
- minikube stop
- minikube status
- minikube delete

## 1.2. Kubectl basic commands

Generic command for check short-names: `kubectl api-resources`

### 1.2.1. Pods

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

### 1.2.2. ReplicaSet

| Description                                            | Command                                       |
| :----------------------------------------------------- | :-------------------------------------------- |
| 1. List replicaSets                                    | `kubectl get rs`                              |
| 2. Describe a replicaSet                               | `kubectl describe rs <replicaSet name>`       |
| 3. Describe in YAML manifesto (check rs configuration) | `kubectl get rs <replicaSet name> -o yaml`    |
| 4. Filter replicaset by label                          | `kubectl get rs -l <label-key>=<label-value>` |

### 1.2.3. Deployment

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

### 1.2.4. Services

| Description                                | Command                                              |
| :----------------------------------------- | :--------------------------------------------------- |
| 1. List services                           | `kubectl get svc`                                    |
| 2. To filter the services                  | `kubectl get svc -l <label-key>=<label-value>`       |
| 3. To describe the service                 | `kubectl describe svc <service-name>`                |
| 4. To get the endpoints                    | `kubectl get endpoints`                              |
| 5. To see service external IP from cluster | `minikube service <service-name> --url -n <ns-name>` |

### 1.2.5. Namespaces

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
| 8. To see limitRange                | `kubectl describe ns <namespace-name>`                                                |

### 1.2.6. Context

| Description                     | Command                                                                                              |
| :------------------------------ | :--------------------------------------------------------------------------------------------------- |
| 1. To get current context       | `kubectl config current-context`                                                                     |
| 2. To see current config        | `kubectl config view`                                                                                |
| 3. To create a context          | `kubectl config set-context <context-name> --namespace=<ns-name> --cluster=minikube --user=minikube` |
| 4. To switch to another context | `kubectl config use-context <context-name>`                                                          |

### 1.2.7. POD limits RAM and CPU

| Description                   | Command                             |
| :---------------------------- | :---------------------------------- |
| 1. To see the use of the NODE | `kubectl get nodes`                 |
| 2. Describe the NODE          | `kubectl describe node <node-name>` |

### 1.2.8. LimitRange

| Description                | Command                                        |
| :------------------------- | :--------------------------------------------- |
| 1. To see limit range      | `kubectl describe limitranges min-max -n prod` |
| 2. Also to see limit range | `kubectl describe ns <namespace-name>`         |

### 1.2.9. ResourceQuota

| Description                   | Command                                        |
| :---------------------------- | :--------------------------------------------- |
| 1. To describe resource quota | `kubectl describe resourcequotas <quota-name>` |

### 1.2.10. ConfigMaps

| Description                                      | Command                                                                      |
| :----------------------------------------------- | :--------------------------------------------------------------------------- |
| 1. To create a configmap                         | `kubectl create configmap <choose-name> --from-file=<configmap-file-path>`   |
| 2. To get the configmap                          | `kubectl get cm`                                                             |
| 3. To describe the configmap                     | `kubectl describe configmaps <configmap-name>`                               |
| 4. To take into the configmap more than one file | `kubectl create configmap <choose-name> --from-file=<configmap-folder-path>` |

### 1.2.11. Secrets

| Description           | Command                                                                      |
| :-------------------- | :--------------------------------------------------------------------------- |
| 1. To create a secret | `kubectl create secret generic <choose-name> --from-file=<secret-file-path>` |
| 2. To get secrets     | `kubectl get secrets`                                                        |
| 3. To describe secret | `kubectl describe secrets <secrets-name>`                                    |

### 1.2.12. PV and PVC

| Description                 | Command           |
| :-------------------------- | :---------------- |
| 1. To get a pvc             | `kubectl get pvc` |
| 2. To get a pv              | `kubectl get pv`  |
| 3. To get the storage class | `kubectl get sc`  |

### 1.2.13. RBAC (Role Base Access Control): Users and Groups

| Description                         | Command                                                                           |
| :---------------------------------- | :-------------------------------------------------------------------------------- |
| 1. To see the certificate authority | `kubectl config view`                                                             |
| 2. To see the cluster info (IP)     | `kubectl cluster-info`                                                            |
| 3. To see cluster config            | `kubectl config view`                                                             |
| 4. To see current contex            | `kubectl config current-context`                                                  |
| 5. To use a context                 | `kubectl config use-context <context-name>`                                       |
| 6. To see if RBAC is enabled        | `kubectl cluster-info dump | grep authorization-mode`                             |
| 7. Enable RBAC in minikube          | `minikube start --vm-drive=none --extra-config=apiserver.authorization-mode=RBAC` |
| 8. To see the roles                 | `kubectl get roles`                                                               |
| 9. To describe a role               | `kubectl describe role <role-name> -n <name-namespace>`                           |
| 10. To get Role Binding             | `kubectl get rolebinding`                                                         |
| 11. To describe Role Binding        | `kubectl describe rolebinding <rolebinding-name>`                                 |
| 12. To see all the cluster roles    | `kubectl get clusterroles`                                                        |

### 1.2.14. RBAC (Role Base Access Control): Service Accounts 

| Description                                  | Command                                     |
| :------------------------------------------- | :------------------------------------------ |
| 1. To get the services accounts              | `kubectl get serviceaccount`                |
| 2. To get the service account of a namespace | `kubectl get sa -n <namespace-name>`        |
| 3. To describe a service account             | `kubectl describe serviceaccount <sa-name>` |
| 4. Service accounts kube system              | `kubectl get sa -n kube-system`             |

### 1.2.15. YAML file

| Description                                         | Command                                                        |
| :-------------------------------------------------- | :------------------------------------------------------------- |
| 1. To apply a yaml manifesto (-f = file)            | `kubectl apply -f pod.yaml`                                    |
| 2. To delete all resources running from a yaml file | `kubectl delete -f pod.yaml`                                   |
| 3. To see command run in the history (CHANGE-CAUSE) | `kubectl rollout history deployment <deployment-name>`         |
| 4. To output in yaml and using grep                 | `kubectl get pod podtest3 -o yaml -n dev | grep -i limits -C3` |

### 1.2.16. Generics

| Description                                | Command                                |
| :----------------------------------------- | :------------------------------------- |
| 1. To edit the configuration of any object | `kubectl edit <object>  <object-name>` |

## 1.3. YAML Notes

### 1.3.1. Pods

#### 1.3.1.1. Labels (Very important)

Metadata to indicate pod metadata in case we have several equal pods but for
different purposes (different app, environment...). Be careful the pod name is
not inside the labels medatadata. Also used by replicaset and deployment for
managing the pods.

#### 1.3.1.2. Owner references

Indicates the parent owner of the replicaSet.

#### 1.3.1.3. Image policy

To check for images locally before looking to remote images:

```yaml
  spec:
    containers:
    - name: backend
      image: k8s-hands-on
      imagePullPolicy: IfNotPresent
```

### 1.3.2. Replicaset

- apiVersion: apps/v1. apps = to understand the prefix run `kubectl api-resources`
and look for NAME = replicaset and you will find the the its GROUP is apps.
- kind: to find the KIND field just do the same as the previous step and find the
KIND field.
- ownerReferences: indicates the parent owner of the replicaSet.

### 1.3.3. Deployment

By default it has .spec.revisionHistoryLimit = 10 (ReplicaSet history limit)

#### 1.3.3.1. CHANGE-CAUSE

It is the cause of the deployment.</br>
To see the deployment CHANGE-CAUSE: `kubectl rollout history deployment <deployment-name>`. </br>
To save command the in the history (CHANGE-CAUSE):

  1. `kubectl rollout history deployment <deployment-name>`.
  2. Add to YAML, after metadata:

        ```yaml
        annotations:
            kubernetes.io/change-cause: "Changes port to 110"
        ```

#### 1.3.3.2. Rollback

The idea is that, for example a pod is not able to start due to a issue, you can
rollback to a previous version by executing:

```bash
kubectl rollout undo deployment <deployment-name> --to-revision=<revision-number>
```

Remember that by default we have 10 old ReplicaSet old version to rollback to,
in case it is necessary.

### 1.3.4. Services

By default TYPE = ClusterIp. It is a virtual Ip that works as the entrance of our pods.
name: *my-service* :arrow_right: It is also the DNS of the service.

#### 1.3.4.1. ClusterIp

Exposes the Service on a cluster-internal IP. Choosing this value makes the
Service only reachable from within the cluster. This is the default `ServiceType`.

```yaml
[...]
  spec:
    type: ClusterIp
```

#### 1.3.4.2. NodePort

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