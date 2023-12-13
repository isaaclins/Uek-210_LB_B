# Anweisungen für minikube
## Starting the minikube:
```
isaac@DESKTOP-TMOK2JO:~/github/K8S-demo$ minikube start
😄  minikube v1.32.0 on Ubuntu 22.04 (amd64)
✨  Using the docker driver based on existing profile
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
🔄  Restarting existing docker container for "minikube" ...
🐳  Preparing Kubernetes v1.28.3 on Docker 24.0.7 ...
🔗  Configuring bridge CNI (Container Networking Interface) ...
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🌟  Enabled addons: storage-provisioner, default-storageclass
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```

## Applying the files:
```
isaac@DESKTOP-TMOK2JO:~/github/K8S-demo$ kubectl apply -f mongo-secret.yaml
secret/mongo-secret unchanged
isaac@DESKTOP-TMOK2JO:~/github/K8S-demo$ kubectl apply -f mongo.yaml
deployment.apps/webapp-deployment unchanged
service/webapp-service unchanged
isaac@DESKTOP-TMOK2JO:~/github/K8S-demo$ kubectl apply -f mongo-config.yaml
configmap/mongo-config unchanged
isaac@DESKTOP-TMOK2JO:~/github/K8S-demo$ kubectl apply -f webapp.yaml
deployment.apps/webapp-deployment unchanged
service/webapp-service unchanged
```

## Get Information:
```
isaac@DESKTOP-TMOK2JO:~/github/K8S-demo$ kubectl get all
NAME                                     READY   STATUS    RESTARTS       AGE
pod/mongo-deployment-7f85cb64d6-8hzbz    1/1     Running   1 (3m5s ago)   18h
pod/webapp-deployment-5db75479c6-rj47g   1/1     Running   1 (3m5s ago)   18h

NAME                     TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
service/kubernetes       ClusterIP   10.96.0.1        <none>        443/TCP          21h
service/mongo-service    ClusterIP   10.111.151.178   <none>        27017/TCP        18h
service/webapp-service   NodePort    10.102.30.233    <none>        3000:30100/TCP   18h

NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/mongo-deployment    1/1     1            1           18h
deployment.apps/webapp-deployment   1/1     1            1           18h

NAME                                           DESIRED   CURRENT   READY   AGE
replicaset.apps/mongo-deployment-7f85cb64d6    1         1         1       18h
replicaset.apps/webapp-deployment-5db75479c6   1         1         1       18h

isaac@DESKTOP-TMOK2JO:~/github/K8S-demo$ kubectl get configmap
NAME               DATA   AGE
kube-root-ca.crt   1      21h
mongo-config       1      18h

isaac@DESKTOP-TMOK2JO:~/github/K8S-demo$ kubectl get secret
NAME           TYPE     DATA   AGE
mongo-secret   Opaque   2      18h
isaac@DESKTOP-TMOK2JO:~/github/K8S-demo$ kubectl get pod
NAME                                 READY   STATUS    RESTARTS        AGE
mongo-deployment-7f85cb64d6-8hzbz    1/1     Running   1 (4m37s ago)   18h
webapp-deployment-5db75479c6-rj47g   1/1     Running   1 (4m37s ago)   18h
```
## Describe
```
isaac@DESKTOP-TMOK2JO:~/github/K8S-demo$ kubectl describe service webapp-service
Name:                     webapp-service
Namespace:                default
Labels:                   <none>
Annotations:              <none>
Selector:                 app=webapp
Type:                     NodePort
IP Family Policy:         SingleStack
IP Families:              IPv4
IP:                       10.102.30.233
IPs:                      10.102.30.233
Port:                     <unset>  3000/TCP
TargetPort:               3000/TCP
NodePort:                 <unset>  30100/TCP
Endpoints:                10.244.0.6:3000
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
```

## Start the webapp
```
isaac@DESKTOP-TMOK2JO:~/github/K8S-demo$ minikube service webapp-service
|-----------|----------------|-------------|---------------------------|
| NAMESPACE |      NAME      | TARGET PORT |            URL            |
|-----------|----------------|-------------|---------------------------|
| default   | webapp-service |        3000 | http://192.168.49.2:30100 |
|-----------|----------------|-------------|---------------------------|
🏃  Starting tunnel for service webapp-service.
|-----------|----------------|-------------|------------------------|
| NAMESPACE |      NAME      | TARGET PORT |          URL           |
|-----------|----------------|-------------|------------------------|
| default   | webapp-service |             | http://127.0.0.1:40765 |
|-----------|----------------|-------------|------------------------|
```
### http://127.0.0.1:40765 will be the url to access the webapp service