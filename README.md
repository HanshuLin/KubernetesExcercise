# KubernetesExcercise
This is a Kubernetes excercise




- Job Template:
apiVersion: batch/v1
kind: Job
metadata:
  name: myjob
spec:
  template:
    metadata:
      name: <name>
    spec:
      containers:
      - name: container-name
        image: perl
        command: ["perl", "-wle", "print \'helloworld\'"]
      restartPolicy: Never
  backoffLimit: 3

kubectl create -f <>.yaml
kubectl scale --replicas=<> job/myjob


- Debugging Error
kubectl logs redis | grep ERROR > output

- Get Persistene Volume
kubectl get pv --sort-by="metadata.name" > pvList

- Add Deployment
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: redis-test-2
  namespace: default
  labels:
    app: redis
  annotations:
    key1: value1
    key2: value2
spec:
  replicas: 3
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
        labels:
          app: redis
    spec:
      containers:
      - name: reids-test-2
        image: redis
        ports:
        - containerPort: 80
Or  <kubectl edit deployment dep> and do

- Create Daemon Set
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
  name: special
spec:
  template:
    metadata:
      name: special
      labels:
        name: special
    spec:
      hostNetwork: true
      hostPID: true
      containers:
        - name: special
          image: nginx
          securityContext:
            privileged: true

- One pod, two containers
apiVersion: v1
kind: Pod
metadata:
  name: twopod
  namespace: default
  labels:
    app: twopod
spec:
  containers:
  - name: redis
    image: redis
    imagePullPolicy: IfNotPresent
    volumeMounts:
    - mountPath: /redis
      name: cachemem
  - name: nginx
    image: nginx
    imagePullPolicy: IfNotPresent
    volumeMounts:
    - mountPath: /nginx
      name: cachemem
  restartPolicy: Always
  volumes:
  - name: cachemem
    emptyDir: {}

- Select node
apiVersion: v1
kind: Pod
metadata:
  name: nostselect
  namespace: default
  labels:
    note: nostselect
spec:
  containers:
  - name: busybox
    image: busybox
    command:
    - sleep
    - "3600"
    imagePullPolicy: IfNotPresent
  nodeSelector:
    disk: ssk


- Update and rollabck
kubectl run nginx-app --image=nginx:1.10-alpha --record
kubectl set image deployment/nginx-app nginx-app=nginx:1.12
kubectl rollout undo deployment/nginx-app --to-revision=1

- Scale an RC
kubectl scale rc aaa --replicas=10


- Stream spec 
kubectl get deployment depy -o jsonpath='{$.spec}' > spec.txt
kubectl get deployment depy -o jsonpath='spec: {$.spec}\n','label: {$.metadata.labels}'
kubectl delete <>

- See label
kubectl get pods -n default -Lrun --selector=run=game > result.txt

- Create Secret
kubectl create secret generic max --from-literal=max=max1
env:
  - name: NAME
    valueFrom: 
      secretKeyRef:
        name: SECRET_NAME
        key: KEY_IN_SECRET


- Pod with volume
apiVersion: v1
kind: Pod
metadata:
  name: question16
  namespace: default
spec:
  containers:
  - name: question16
    image: busybox
    command:
    - sleep
    - "3600"
    imagePullPolicy: IfNotPresent
    volumeMounts:
    - mountPath: /path
      name: vol
  volumes:
  - name: vol
    emptyDir: {}

- Ingress
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: ingress-demo
spec:
  rules:
  - host: ingress.demo.com
    http:
      paths:
      - backend:
          serviceName: ghost
          servicePort: 80

- CPU Usage
kubectl top pods --namespace=kube-system


- DNS between services
Expose nginx service first
kubectl exec <> -- nslookup nginx-service > nsconfig.txt

- Drain node
kubectl drain node <>
 
- Install K8s from tar.gz
TOO LONG

- Persistent Volume
kind: PersistentVolume
apiVersion: v1
metadata:
  name: task-pv-volume
  labels:
    type: local
spec:
  storageClassName: manual
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/tmp/data"



