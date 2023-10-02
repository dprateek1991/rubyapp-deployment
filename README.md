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

Go to helm-chart folder and run 

```
make install
```

In a separate terminal, start minikube tunnel

```
minikube tunnel
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