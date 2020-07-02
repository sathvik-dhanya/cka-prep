#### Create an nginx pod that mounts an empty directory at /usr/share/nginx/html
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: with-volume
spec:
  containers:
  - name: nginx
    image: nginx
    volumeMounts:
    - mountPath: /usr/share/nginx/html
      name: web-path
  volumes:
  - name: web-path
    emptyDir: {}
```

#### Create a pod with two containers:  An nginx container and a busybox container.
##### The nginx container should be named websrv and should mount a shared empty directory at /usr/share/nginx/html. The busybox container should be named dolittle and should mount the shared directory at /webroot.  It should print "Up and running" to the console and then sleep for 5 minutes (300 seconds)
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: with-volume
spec:
  containers:
  - name: nginx
    image: nginx
    volumeMounts:
    - mountPath: /usr/share/nginx/html
      name: shared-directory
  - name: busybox
    image: busybox
    command: ["/bin/sh", "-c", "echo Up and Running && sleep 300"]
    volumeMounts:
    - mountPath: /webroot
      name: shared-directory
  volumes:
  - name: shared-directory
    emptyDir: {}
```

#### Create a job that creates an empty file at /home/centos/fromk8s/done on the node the pod is run on
```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: create-done
spec:
  template:
    spec:
      containers:
      - name: busybox
        image: busybox
        command: ["/bin/sh", "-c", "touch /hostpath/done"]
        volumeMounts:
        - mountPath: /hostpath
          name: node-dir
      restartPolicy: Never
      volumes:
      - name: node-dir
        hostPath: 
          path: /home/centos/fromk8s
          type: DirectoryOrCreate
```

#### Create an deployment that creates 3 pods with an image of nginx.  The pods should each mount a volume on their respective hosts at the /home/centos/task06.  If this directory is not present on the host, it should be created.  This directory should be mounted in the containers at /usr/share/nginx/html
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: websrv-with-hostpath
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        volumeMounts:
        - mountPath: /usr/share/nginx/html
          name: data-volume
      volumes:
      - name: data-volume
        hostPath:
          path: /home/centos/task06
          type: DirectoryOrCreate
```

#### Create a deployment with an init container.  The deployment should have 2 replicas. The application container should use an nginx image to serve a web page. The init container should use a busybox image to create a file called index.html at the web root that contains the plain text "Created by K8s Init Container"
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
    name: initcontainer
    labels:
        app: nginx
spec:
    replicas: 2
    selector:
        matchLabels:
            app: nginx
    template:
        metadata:
            labels:
                app: nginx
        spec:
            containers:
            - name: nginx-container
              image: nginx
              ports:
              - containerPort: 80
              volumeMounts:
              - mountPath: /usr/share/nginx/html
                name: site-volume
            initContainers:
            - name: write-the-data
              image: busybox
              command: ['sh', '-c', 'echo Created by K8s Init Container
 > /site/index.html']
              volumeMounts:
              - mountPath: /site
                name: site-volume
            volumes:
            - name: site-volume
              emptyDir: {}
```

#### Create a ConfigMap and a deployment that uses the ConfigMap. The application container should use an nginx image to serve a web page. The ConfigMap should create a text file at the web root that contains the plain text "Created by K8s ConfigMap"
```yaml
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: mynewindex
data:
  index.html: |-
    Created by K8s ConfigMap
---
apiVersion: apps/v1
kind: Deployment
metadata:
    name: configmapexercise
    labels:
        app: nginx
spec:
    replicas: 2
    selector:
        matchLabels:
            app: nginx
    template:
        metadata:
            labels:
                app: nginx
        spec:
            containers:
            - name: nginx-container
              image: nginx
              ports:
              - containerPort: 80
              volumeMounts:
              - mountPath: /usr/share/nginx/html
                name: site-volume
            volumes:
            - name: site-volume
              configMap: 
                name: mynewindex
```

