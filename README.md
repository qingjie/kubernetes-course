# Kubernetes course
This repository contains the course files for my Kubernetes course on Udemy: https://www.udemy.com/learn-devops-the-complete-kubernetes-course/?couponCode=KUBERNETES_GITHUB
---

```
docker --version
docker build .
docker run -p 3000:3000 -it <image-id>
curl localhost:3000
```

```
docker login
docker images
docker tag <image-id> your-login/docker-demo
docker tag <image-id> your-login/docker-demo:latest
docker push your-login/docker-demo


https://12factor.net/

# get information about all running pods
kubectl get pod
# descibe one pod
kubectl describe pod <pod>
# Expose the port of a pod(creates a new service)
kubectl expose pod <pod> --port=444 --name=frontend
# port forward the exposed pod port to your local machine
kubectl port-forward <pod> 8080
# attach to the pod
kubectl attach <podname> -i
# Execute a command on the pod
kubectl exec <pod> -- command
# add a new label to a pod
kubectl lable pods <pod> mylabel=awesome
# Run a shell in a pod -very useful for debugging
kubectl run -i --tty busybox --image=busybox --restart=Never --sh
```

```
git clone https://github.com/qingjie/kubernetes-course
cd kubernetes-course
kubectl get node
cat first-app/hello-world.yaml
kubectl -f first-app/hello-world.yaml
kubectl get pod
kubectl describe pod <pod>

kubectl port-foward <pod> 8081:3000
curl localhost:8081
or
kubectl expose pod <pod> --type=NodePortn --name <pod>-service
kubectl get service
```

```
cat kubernetes-course/replication-controller/helloworld-repl-controller.yaml
kubectl create -f  kubernetes-course/replication-controller/helloworld-repl-controller.yaml
kubectl get pods
kubectk describe pod <pod>
kubectl delete pod <pod>
# it will have 4 pods
kubectl scale --replicas=4 -f kubernetes-course/replication-controller/helloworld-repl-controller.yaml
kubectl get rc
# set replica = 1
kubectl scale --replicas=1 rc/replication-controller
kubectl get pods
# delete replica
kubectl delete rc/replication-controller

```

```
# Get information on current deployments
kubectl get deployments
# Get information about the replica sets
kubectl get rs
# get pods, and also show labels attached to those pods
kubectl get pods --show-labels
# Get deployment status
kubectl rollout status deployment/helloworld-deployment
# Run k8s-demo with the image label version 2
kubectl set image deployment/helloworld-deployment k8s-demo=k8s-demo:2
# Edit the deployment object
kubectl edit deployment/helloworld-deployment 
# Get status of the rollout
kubectl rollout status deployment/helloworld-deployment 
# Get the rollout history
kubectl rollout history deployment/helloworld-deployment 
# Rollback to previous version
kubectl rollout undo deployment/helloworld-deployment 
# Rollback to any version
kubectl rollout undo deployment/helloworld-deployment --to-revision=n
```
