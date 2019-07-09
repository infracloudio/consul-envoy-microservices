# consul-envoy-microservices

### Creating Service Mesh Demo

This demo provides a simplified approach for creating service mesh in conventional microservices running on a Kubernetes cluster.
This demo uses [Hipster Shop: Cloud-Native Microservices Demo Application](https://github.com/GoogleCloudPlatform/microservices-demo)

**Pre-requisite**

Deploy a Consul cluster on your Kubernetes cluster using Helm-chart

Correctly set the following parameters in `values.yml` for incorporating Consul connect features:

```
global:
  enabled: true
  domain: consul

server:
  enabled: true
  connect: true

client:
  enabled: true
  grpc: true

dns:
  enabled: true

ui:
  enabled: true
  service:
    enabled: true
    type: NodePort

connectInject:
  enabled: true
  default: false
  ```

To deploy service mesh cluster run
`kubectl apply -f Kubernetes-yamls/.`

run `kubectl get pods` to check whether all services are up and running

```
NAME                                                              READY   STATUS    RESTARTS   AGE
adservice-fc748b566-d57z2                                         2/2     Running   0          20m
cartservice-ffc5b8dd8-w6hbc                                       2/2     Running   1          20m
checkoutservice-5955b99f56-p2g4s                                  2/2     Running   0          20m
currencyservice-f895755-r7xqf                                     1/2     Running   2          20m
emailservice-b84f54dcf-zwltv                                      2/2     Running   0          20m
frontend-56cb8675f4-b9zpk                                         2/2     Running   0          20m
loadgenerator-77789f946f-5677g                                    1/1     Running   0          20m
paymentservice-7456c67577-kzctk                                   2/2     Running   0          20m
productcatalogservice-7cd8b789bf-wcjk9                            2/2     Running   0          20m
recommendationservice-9dcf5884c-xnfjg                             2/2     Running   0          20m
redis-cart-5b6b9f6b8c-dqj4b                                       1/1     Running   0          20m
riotous-magpie-consul-connect-injector-webhook-deployment-5jxkl   1/1     Running   1          31h
riotous-magpie-consul-lmfn5                                       1/1     Running   1          31h
riotous-magpie-consul-server-0                                    1/1     Running   1          31h
shippingservice-5b5b5d8dcb-5lgnd                                  2/2     Running   0          20m
```

run `kubectl port-forward frontend-56cb8675f4-b9zpk 8080:8080` to access the Hipster web page on [localhost:8080]

run `kubectl port-forward riotous-magpie-consul-server-0 8500:8500` to access consul-ui on [localhost:8500]

Click on services and check that all the services and their respective side-cars are registered with consul

Click on the Intentions in Consul-ui and create a new Intention by selecting 'All Services(*)' 
in source and destination and provide the rule to deny traffic between them.

As you can see on the Hipster web page, some pages are not loaded as traffic is denied between those services.

Communication between services is enabled through Consul-Envoy proxies and not using conventional services
