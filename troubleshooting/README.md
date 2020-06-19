# Troubleshooting (10%)

kubernetes.io > Documentation > Reference > kubectl CLI > [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

kubernetes.io > Documentation > Tasks > Monitor, Logging & Debugging > [Determine the Reason for Pod Failure](https://kubernetes.io/docs/tasks/debug-application-cluster/determine-reason-pod-failure/)

kubernetes.io > Documentation > Tasks > Monitor, Logging & Debugging > [Application Introspection and Debugging](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-application-introspection/)

kubernetes.io > Documentation > Tasks > Monitor, Logging & Debugging > [Debug Services](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-service/)

kubernetes.io > Documentation > Tasks > Monitor, Logging & Debugging > [Troubleshoot Applications](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-application/)

kubernetes.io > Documentation > Tasks > Monitor, Logging & Debugging > [Troubleshoot Clusters](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-cluster/)

#### Troubleshooting tips
**Pods**

`kubectl get pod -o wide` - get running pods and their status

`kubectl logs <pod_name>` - get stdout/stderr from a pod

`kubectl describe pod <pod_name>` - detailed information about a pod, and it's events

**Services**

`kubectl get svc` & `kubectl describe svc <svc_name>` to get more information

When troubleshooting services, particularly those which are only accessible internally, spin up a pod to which you can exec into to run tests.

`kubectl run -it busybox --image=busybox -- sh`

Here you can use tools such as ping, telnet etc.

**Deployments**

`kubectl get deployment` & `kubectl describe deployment <dep_name>` to get more information

**Control plane**
```bash
kubectl get componentstatuses

NAME                 STATUS    MESSAGE             ERROR
controller-manager   Healthy   ok                   
scheduler            Healthy   ok                   
etcd-0               Healthy   {"health":"true"} 
...
...
```

