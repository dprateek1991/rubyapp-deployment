### Build and Push the docker image

```
docker build -t datawavelabs/ruby-app:latest .
docker push -t datawavelabs/ruby-app:latest
```

### Run the ruby code on local or as a container

```
ruby web_server.rb
docker run -p 80:80 datawavelabs/ruby-app:latest
```

### Test the code 

```
http://127.0.0.1/
http://127.0.0.1/healthcheck
```

### Deploy the Helm Chart

Start Minikube Cluster

```
minikube start
minikube stop
```

Create ruby namespace

```
kubectl create ns ruby-app
```

Create docker secret

```
kubectl -n ruby-app create secret docker-registry docker-login --docker-server=https://index.docker.io/v1/ --docker-username= --docker-password= --docker-email=
```

Go to helm-chart folder and run 

```
make install
```

Check application pods
```
kubectl -n ruby-app get pods
NAME                       READY   STATUS    RESTARTS   AGE
ruby-app-5f986899f-f7h4h   1/1     Running   0          27s
ruby-app-5f986899f-j9xzg   1/1     Running   0          27s
```

In a separate terminal, start minikube tunnel

```
minikube tunnel
```

Check the K8s service
```
kubectl -n ruby-app get svc
NAME       TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
ruby-app   LoadBalancer   10.101.125.53   127.0.0.1     80:30879/TCP   79s
```

Check your service at 

```
http://127.0.0.1/
http://127.0.0.1/healthcheck
```

### Install ArgoCD 

```
kubectl create ns argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/v2.5.8/manifests/install.yaml
```

Access ArgoCD server via port-forwarding

```
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

```
127.0.0.1:8080
```

Get password as 

```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```

### Run Argocd application

```
kubectl apply -f ruby-app.yaml
```