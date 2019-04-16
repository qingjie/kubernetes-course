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

```
# change wardviaene to qingjiezhao
cat deployment/helloworld.yml
kubectl create -f deployment/helloworld.yml
kubectl get deployments
kubectl get rs
kubectl get pods
kubectl get pods --show-labels
kubectl rollout status deployments/helloworld-deployment
kubectl expose deployment helloworld-deployment --type=NodePort
kubectl get service
kubectl describe service helloworld-deployment
kubectl set image deployment/helloworld-deployment k8-demo=qingjiezhao/k8s-demo:2
kubectl rollout status deployment/helloworld-deployment
kubectl rollout history deployment/helloworld-deployment
kubectl rollout undo deployment/helloworld-deployment
kubectl rollout status deployment/helloworld-deployment
# set revisionHistoryLimit: 10
kubectl edit deployment/helloworld-deployment

kubectl set image deployment/helloworld-deployment k8s-demo=qingjiezhao/k8s-demo:2
kubectl rollout history deployment/helloworld-deployment
kubectl set image deployment/helloworld-deployment k8s-demo=qingjiezhao/k8s-demo:1
kubectl rollout history deployment/helloworld-deployment
kubectl rollout undo deployment/helloworld-deployment --to-revision=2
kubectl rollout history deployment/helloworld-deployment
```

```
cat deployment/helloworld-nodeselector.yml
ubectl get nodes --show-labels
kubectl get pods
kubectl describe pod helloworld-deployment-6655b48c56-54ppl

kubectl get nodes
kubectl label nodes ip-172-20-33-13.ec2.internal hardware=high-spec
kubectl get nodes --show-labels




cat deployment/helloworld-healthcheck.yml
kubectl create -f deployment/helloworld-healthcheck.yml
kubectl get pods
kubectl describe pod helloworld-deployment-689656bb4d-cmdjj
kubectl edit deployment/helloworld-deployment
```

```

kubectl create -f helloworld-healthcheck.yml
kubectl get pods
kubectl create -f helloworld-liveness-readiness.yml
kubectl get pods


brew install watch
kubectl create -f lifecycle.yaml
kubectl exec -it lifecycle-57596644f4-k7vll -- cat /timing
kubectl exec -it lifecycle-57596644f4-k7vll -- tail /timing -f
```

```
cat deployment/helloworld-secrets.yml
kubectl create -f deployment/helloworld-secrets.yml

cat deployment/helloworld-secrets-volumes.yml
kubectl create -f deployment/helloworld-secrets-volumes.yml

kubectl get pods
kubectl describe pod/helloworld-deployment-596cb56f6d-55fbt

#get in the shell of docker
kubectl exec helloworld-deployment-596cb56f6d-55fbt -i -t -- /bin/bash
cat /etc/creds/username
cat /etc/creds/password
mount
ls /run/secrets/kubernetes.io/serviceaccount/
```

```
kubectl create -f wordpress/wordpress-secrets.yml
kubectl create -f wordpress/wordpress-single-deployment-no-volumes.yml
kubectl get pods
kubectl describe pod wordpress-deployment-9cb886cf6-dr9kl

cat README.md
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml


kubectl create -f sample-user.yaml
kubectl -n kube-system get secret | grep admin-user
kubectl -n kube-system describe secret admin-user-token-<id displayed by previous command>
https://api.test.operr.net/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/#!/storageclass?namespace=default
```

```
cat configmap/reverseproxy.conf
kubectl create configmap nginx-config --from-file=configmap/reverseproxy.conf
kubectl get configmap
kubectl get configmap nginx-config -o yaml
cat configmap/nginx.yml
kubectl create -f configmap/nginx.yml
kubectl create -f configmap/nginx-service.yml
kubectl exec -i -t helloworld-nginx -c nginx -- bash
cat /etc/nginx/conf.d/reverseproxy.conf
```

```
aws ec2 create-volume --size 10 --region us-east-1 --availability-zone us-east-1a --volume-type gp2 --tag-specifications 'ResourceType=volume, Tags=[{Key=KubernetesCluster,Value=test.qingjiezhao.net}]'

# add vol-02379fe282d21a7e3
vi volumes/helloworld-with-volume.yml

kubectl create -f volumes/helloworld-with-volume.yml
kubectl get pod
kubectl describe pod helloworld-deployment-687844d5c4-mzkjz
kubectl exec helloworld-deployment-687844d5c4-mzkjz -i -t -- bash
ls -ahl /myvol/
echo 'test' > /myvol/myvol.txt
echo 'test2' > /test.txt
ls -ahl /myvol/myvol.txt
ls -ahl /test.txt
exit

#get node info when kubectl describe pod helloworld-deployment-687844d5c4-mzkjz
kubectl drain ip-172-20-33-13.ec2.internal --force
kubectl get pod
kubectl describe pod helloworld-deployment-687844d5c4-czt78
kubectl exec helloworld-deployment-687844d5c4-czt78 -i -t -- bash
ls -ahl /myvol/
ls -ahl /myvol/myvol.txt
ls -ahl /test.txt

# remove volume 
vi volumes/helloworld-with-volume.yml
aws ec2 delete-volume --volume-id vol-02379fe282d21a7e3
kubectl delete -f volumes/helloworld-with-volume.yml
```
