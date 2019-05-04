# Kubernetes & Selenium 
A lot of digital transformations are going on into Quality Assurance. Companies are focusing more on how they can automate their test processes to make them quicker, easier to configure and more efficiently deployed. To achieve this  goals we are going to use Kubernetes orchestration capabilities and Selenium Grid automation management.

## Kubernetes 101 
<img height="190" width="210" src="https://github.com/twogg-git/k8s-selenium/blob/master/imgs/kubernetes.png">

- **Pod:** group of one or more running containers.    
- **Pod template:**  describes the desired state of a single container.     
- **Labels:** key-value pair attached to each object in Kubernetes to group the resources together.   
- **Replication Controller:**  pod template + number of desired replicas = Replication controller.   
- **Services:** provide abstracted static IP address for pods selected by labels   

## Selenium Grid
<img height="80" width="300" src="https://raw.githubusercontent.com/twogg-git/k8s-selenium/master/imgs/selenium.png">

When it comes to web-application automation, Selenium Grid has become the first fundamental part of the revolutionizing tool sets, allowing to perform Selenium tests on different machines and with multiple browsers at the same time.

- **Selenium Hub:** The hub-and-node concept has a serious weak spot, Selenium Hub is a single browser access point that makes all nodes unavailable when the hub goes down, just be sure to check the hub first.

## Hands ON!

As always we are going to use an online ready to use single node cluster instance from Katacoda. 
https://www.katacoda.com/courses/kubernetes/launch-single-node-cluster

**Note:** All commands used in this hands on are compatible with your local minikube or kubernetes instance.

```sh
minikube start
```

The version that we are going to use is Selenium Grid 3.3.0 https://www.seleniumhq.org/download/
```sh
kubectl run selenium-hub --image selenium/hub:3.3.0 --port 4444
```

If you are using a public minikube instance (katacoda) expose the deployment with **--external-ip=$(minikube ip)** flag. For local instances ignore the external ip flag.
```sh
kubectl expose deployment selenium-hub --external-ip=$(minikube ip) --type=NodePort
```

```sh
kubectl get services
```

```sh
minikube service selenium-hub --url
```

```sh
Click + "Select port to view on Host 1" + Port XXXXX
```

```sh
kubectl run selenium-node-chrome --image selenium/node-chrome:3.3.1 --env="HUB_PORT_4444_TCP_ADDR=selenium-hub" --env="HUB_PORT_4444_TCP_PORT=4444"
```

```sh
kubectl run selenium-node-firefox --image selenium/node-firefox:3.3.1 --env="HUB_PORT_4444_TCP_ADDR=selenium-hub" --env="HUB_PORT_4444_TCP_PORT=4444"
```

```sh
kubectl scale deployment selenium-node-chrome --replicas=3
```

```sh
kubectl scale deployment selenium-node-firefox --replicas=2
```

```sh
kubectl describe pod selenium-hub
```

```sh
kubectl describe pod selenium-node-chrome
```

```sh
kubectl get pods
```

```sh
kubectl exec selenium-hub-xxxxxx -- printenv
```

```sh
kubectl exec selenium-node-chrome-xxxxxx -- printenv
```

```sh
kubectl exec selenium-node-chrome-xxxxxx --printenv
```
