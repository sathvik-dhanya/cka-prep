#### Create a namespace called 'mynamespace' and a pod with image nginx called nginx on this namespace (using kubectl command)
```bash
kubectl create namespace mynamespace
kubectl run nginx --image=nginx -n mynamespace
```

#### Same (using yaml)
```bash
kubectl run nginx --image=nginx --dry-run=client -o yaml > pod.yaml
```
OR
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
  restartPolicy: Never
```

#### Create a busybox pod (using kubectl command) that runs the command "env". Run it and see the output
```bash
kubectl run busybox --image=busybox --command --restart=Never -it -- env # -it will help in seeing the output
# or, just run it without -it
kubectl run busybox --image=busybox --command --restart=Never -- env
# and then, check its logs
kubectl logs busybox
```

#### Same (using yaml)
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: busybox
spec:
  containers:
  - command:
    - env
    image: busybox
    name: busybox
```
```bash
# apply it and then see the logs
kubectl apply -f pod.yaml
kubectl logs busybox
```

#### Get the YAML for a new ResourceQuota called 'myrq' with hard limits of 1 CPU, 1G memory and 2 pods without creating it
```bash
kubectl create quota myrq --hard=cpu=1,memory=1G,pods=2 --dry-run=client -o yaml
```

#### Create an nginx pod allow traffic on port 80
```bash
kubectl run nginx --image=nginx --port=80
```

#### Change pod's image to nginx:1.7.1
```bash
kubectl edit pods nginx
# change image from nginx to nginx:1.7.1
OR
kubectl set image pod/nginx nginx=nginx:1.7.1
```

#### Get nginx pod's ip created in the previous step, use a temp busybox image to wget its '/'
```bash
kubectl get pods -o wide # grab IP of nginx pod
kubectl run busybox --image=busybox --rm -it --restat=Never -- wget -O- IP
```

#### Create a busybox pod that echoes 'hello world' and then exits
```bash
kubectl run busybox --image=busybox --rm -it --restart=Never -- echo 'hello world'
kubectl run busybox --image=busybox --rm -it --restart=Never -- /bin/sh -c 'echo hello world' 
```

#### Create an nginx pod with an env value as 'var=val'
```bash
kubectl run nginx --image=nginx --env=var1=val1 -it --rm -- env
OR
# create pod and check env var in a separate step
kubectl run nginx --image=nginx --env=var=val
kubectl describe pod nginx | grep val
kubectl exec -it nginx -- env
kubectl exec -it nginx -- /bin/bash -c 'echo $var'
```
