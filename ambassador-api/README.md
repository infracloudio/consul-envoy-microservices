# Ambassador API Gateway for Consul Service Mesh in K8s

1. Running Application
```
kubectl apply -f ambassador/.
```
```
kubectl apply -f .
```

2. Check the Running Application
- In case of Minikube, RUN the following command to the ip of service
```
minikube service list
```
- Copy the URL of `consul-frontend` service to access the app in browser.