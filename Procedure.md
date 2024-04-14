1. Pull docker image used in fast_deployment.yaml in your local machine and load into MiniKube VM,

Docker Pull Command for image tmr_api:v1:

docker pull xalien073/tmr_api:v1

MiniKube command to load image:

minikube image load tmr_api:v1

OR

edit fast_deployment.yaml's image value to fetch from remote repository.

2. To deploy our app on k8s cluster, in this case local MiniKube use the following commands:

kubectl apply -f fast_deployment.yaml

kubectl apply -f fast_service.yaml

3. To access our app inside MiniKube cluster use the following commands:

kubectl get pods -o wide

above command will show pod's IP, copy it.

minikube ssh

with this command you'll login into minikube vm

curl http://obtained_IP:8000

this command will check for our app is up and running or not,
replace obtained_IP with pod's IP we retrieved earlier.

4. To access outside cluster, follow the commands given below:

A. Using service tunnel:

minikube service fastapi-service

B. Using NodePort:

kubectl get service fastapi-service -o jsonpath='{.spec.ports[0].nodePort}'

note the NodePort retrievd with the above commnad,

minikube ip

with this command you'll get ip address of your minikube vm,
now with this we build URL to access our app externaly.

URL = http://minikube_ip:NodePort

C. Using port forwarding:

kubectl port-forward <pod-name> <local-port>:8000

for example,
kubectl port-forward fastapi-app-5486756cc7-wqwqz 8000:8000
