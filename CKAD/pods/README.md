<<<<<<< HEAD
minikube start --driver=virtualbox
List the currently supported addons:
 minikube addons list
Enable an addon, for example, metrics-server:
minikube addons enable metrics-server
Disable metrics-server:
 minikube addons disable metrics-server

minikube start      ///starts local 1 node cluster
minikube dashboard
kubectl run hello-minikube --image=gcr.io/google_containers/echoserver:1.4 --port=8080
kubectl expose deployment hello-minikube --type=NodePort

kubectl get podView the Pod and Service you just created:
 kubectl version
kubectl get pod,svc -n kube-system
kubectl cluster-info

curl $(minikube service hello-minikube --url)
kubectl delete deployment hello-minikube
minikube stop

##################

kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.10

To access the hello-minikube Deployment, expose it as a Service:
kubectl expose deployment hello-minikube --type=NodePort --port=8080

Check if the Pod is up and running:
kubectl get pod

get all running services
kubectl get --all-namespaces services

Similarly, you can take a look at the set of pods that were created during cluster startup.
 kubectl get --all-namespaces pods

Get the URL of the exposed Service to view the Service details:
minikube service hello-minikube --url

Delete the hello-minikube Service:
kubectl delete services hello-minikube

Delete the hello-minikube Deployment:
kubectl delete deployment hello-minikube

minikube stop
minikube delete


####################


kubectl get pods
kubectl get pods [pod name]
kubectl expose <type name> <identifier/name> [—port=external port] [—target-port=container-port [—type=service-type]
kubectl expose deployment tomcat-deployment --type=NodePort
kubectl port-forward <pod name> [LOCAL_PORT:]REMOTE_PORT]
kubectl attach <pod name> -c <container>
kubectl exec [-it] <pod name> [-c CONTAINER] — COMMAND [args…]
kubectl exec -it <pod name> bash
kubectl label [—overwrite] <type> KEY_1=VAL_1 ….
kubectl label pods <pod name> healthy=fasle
kubectl run <name> —image=image
kubectl run hazelcast --image=hazelcast/hazelcast --port=5701 # the hazelcast docker image has been moved to hazelcast/hazelcast (https://hub.docker.com/r/hazelcast/hazelcast
kubectl describe pod


#Scaling Commands
kubectl scale --replicas=4 deployment/tomcat-deployment
kubectl expose deployment tomcat-deployment --type=NodePort
kubectl expose deployment tomcat-deployment --type=LoadBalancer --port=8080 --target-port=8080 --name tomcat-load-balancer
kubectl describe services tomcat-load-balancer
=======
kubectl version

Minikube commands
minikube start///starts local 1 node cluster

kubectl run hello-minikube --image=gcr.io/google_containers/echoserver:1.4 --port=8080

kubectl expose deployment hello-minikube --type=NodePort

kubectl get pod

curl $(minikube service hello-minikube --url)

kubectl delete deployment hello-minikube

minikube stop


>>>>>>> 9436a58f55d7212d0d0f0160bc9776a7836723d9

# Practice Test - PODs

## Exercises

**How many PODs exist on the system?**

> in the current(default) namespace

`kubectl get pods`

**Create a new pod with the NGINX image**

> Image name: nginx

`kubectl run nginx --image=nginx`

**How many pods are created now?**

`kubectl get pods --no-headers | wc -l`

**What is the image used to create the new pods?**

`kubectl describe pod <pod_name>`

**Which nodes are these pods placed on?**

`kubectl get pods -o wide`

**How many containers are part of the pod 'webapp'?**

`kubectl describe pod webapp`

**Delete the 'webapp' Pod.**

`kubectl delete pod webapp`

**Create a new pod with the name 'redis' and with the image 'redis123'**

Use a pod-definition YAML file. And yes the image name is wrong!

> Name: redis, Image Name: redis123

`kubectl run redis --image=redis123 --dry-run=client -o yaml > redis.yaml`

`kubectl create -f redis.yaml`

Now fix the image on the pod to 'redis'.

`kubectl apply -f redis.yaml`

**Creating Pods**

Set up an nginx web server in their Kubernetes cluster. The nginx server will need to be accessible via network in the future, so you will need to expose port 80 as a containerPort for the nginx container. Your team has also asked you to ensure that nginx runs in quiet mode for the time being to cut down on unnecessary log output. You can do this by setting the command to nginx and passing the following arg to the container: -g daemon off; -q. As this nginx server belongs to the Web team, you will need to create it in the team's web namespace.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  namespace: web
spec:
  containers:
  - name: nginx
    image: nginx
    command: ["nginx"]
    args: ["-g", "daemon off;", "-q"]
    ports:
    - containerPort: 80
```







###########################################################

apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-nginx
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 4
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.15.0
        ports:
        - containerPort: 80
        livenessProbe:
          httpGet:
            path: /
            port: 80
          initialDelaySeconds: 30
          periodSeconds: 30
        readinessProbe:
          httpGet:
            path: /
            port: 80
          initialDelaySeconds: 15
          periodSeconds: 3
      # nodeSelector:
      #   storageType: ssd
      
 

apiVersion: v1
kind: Service
metadata:
  name: my-nginx
  labels:
    run: my-nginx
spec:
  ports:
  - port: 80
    protocol: TCP
  selector:
    run: my-nginx 
      


 kubectl apply -f ./nginx-deployment.yaml
 kubectl expose deployment nginx-deployment --type=NodePort # same as running kubectl create -f nginx-service.yaml
 minikube service nginx-deployment --url
 curl <URL_FROM_PREVIOUS_COMAND>
 kubectl describe pod nginx-deployment-56b8664f97-bj45s


kubectl proxy 
and navigate to http://localhost:8001/ui
Scale an existing deployment
 kubectl scale --replicas=4 deployment/nginx-deployment
Update the service for the 4 replicas:
 kubectl expose deployment nginx-deployment --type=LoadBalancer --port=8080 --target-port=8080 --name nginx-load-balancer$ kubectl describe services nginx-load-balancer
