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
<img height="65" width="300" src="https://raw.githubusercontent.com/twogg-git/k8s-selenium/master/imgs/selenium.png">

When it comes to web-application automation, Selenium Grid has become the first fundamental part of the revolutionizing tool sets, allowing to perform Selenium tests on different machines and with multiple browsers at the same time.

- **Selenium Hub:** The hub-and-node concept has a serious weak spot, Selenium Hub is a single browser access point that makes all nodes unavailable when the hub goes down, just be sure to check the hub first.

## Hands ON!

As always we are going to use an online ready to use single node cluster instance from Katacoda. 
https://www.katacoda.com/courses/kubernetes/launch-single-node-cluster

**Note:** All commands used in this hands on are compatible with your local minikube or kubernetes instance.

```sh
minikube start
```
<img src="https://github.com/twogg-git/k8s-selenium/blob/master/imgs/minikube_start.png">

The version that we are going to use is Selenium Grid 3.3.0 https://www.seleniumhq.org/download/
```sh
kubectl run selenium-hub --image selenium/hub:3.3.0 --port 4444
```
<img src="https://github.com/twogg-git/k8s-selenium/blob/master/imgs/run_selenium.png">

If you are using a public minikube instance (katacoda) expose the deployment with **--external-ip=$(minikube ip)** flag. For local instances ignore the external ip flag.
```sh
kubectl expose deployment selenium-hub --external-ip=$(minikube ip) --type=NodePort
```
<img src="https://github.com/twogg-git/k8s-selenium/blob/master/imgs/deploy_selenium.png">

```sh
kubectl get services
```
<img src="https://github.com/twogg-git/k8s-selenium/blob/master/imgs/get_services.png">

```sh
minikube service selenium-hub --url
```
<img src="https://github.com/twogg-git/k8s-selenium/blob/master/imgs/selenium_url.png">

```sh
Click + "Select port to view on Host 1" + Port 4444
```
<img src="https://github.com/twogg-git/k8s-selenium/blob/master/imgs/selenium_console.png">

```sh
kubectl run selenium-node-chrome --image selenium/node-chrome:3.3.1 --env="HUB_PORT_4444_TCP_ADDR=selenium-hub" --env="HUB_PORT_4444_TCP_PORT=4444"

kubectl run selenium-node-firefox --image selenium/node-firefox:3.3.1 --env="HUB_PORT_4444_TCP_ADDR=selenium-hub" --env="HUB_PORT_4444_TCP_PORT=4444"
```
<img src="https://github.com/twogg-git/k8s-selenium/blob/master/imgs/create_pods.png">

<img src="https://github.com/twogg-git/k8s-selenium/blob/master/imgs/console_with_pods.png">

```sh
kubectl scale deployment selenium-node-chrome --replicas=3

kubectl scale deployment selenium-node-firefox --replicas=2
```
<img src="https://github.com/twogg-git/k8s-selenium/blob/master/imgs/console_scalated.png">

```sh
kubectl describe pod selenium-node-chrome
kubectl describe pod selenium-hub
```
<img src="https://github.com/twogg-git/k8s-selenium/blob/master/imgs/describe_deployment.png">

```sh
kubectl get pods
```
<img src="https://github.com/twogg-git/k8s-selenium/blob/master/imgs/get_pods.png">

```sh
kubectl exec selenium-hub-xxxxxx --printenv
kubectl exec selenium-node-chrome-xxxxxx --printenv
```
<img src="https://github.com/twogg-git/k8s-selenium/blob/master/imgs/printenv_pod.png">
