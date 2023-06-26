# Used Virtualbox VM Ubuntu 20.04 LTS

Config: Memory: 6 GB Disk: 40GB CPU: 4

## Install minikube and docker

```shell
apt install -y curl wget apt-transport-https
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
install minikube-linux-amd64 /usr/local/bin/minikube
minikube version

sudo apt install apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update

sudo apt install docker-ce docker-ce-cli containerd.io

minikube start --addons=ingress --cpus=4 --cni=flannel --install-addons=true --kubernetes-version=stable --memory=5g --driver=docker --force

minikube addons enable metrics-server


NOTE DOWN: minikube ip

```


## Install Prometheus using Helm package manager:

```shell
curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get-helm-3 > get_helm.sh
chmod 700 get_helm.sh
./get_helm.sh

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo list
helm repo update
helm search repo prometheus-community
helm search repo prometheus-community | grep kube-prometheus

helm install monitoring prometheus-community/kube-prometheus-stack
```

## Deploying the Tomcat Application

## Under the tomcat directory, build the Tomcat welcome application:

```shell
apt install maven

mvn clean package

Build the Docker image:

docker build -t haptiktomcat .


Run the Docker container:

docker run -d -p 8080:8080 haptiktomcat

eval $(minikube docker-env)

```


## Configuring Nginx and Filebeat

```shell

Create ConfigMaps:

kubectl create configmap nginx-config --from-file=nginx.conf
kubectl create configmap filebeat-config --from-file=filebeat.yml


Create kibana. elasticsearch, logstash, filebeat

I have created folder with respect to service  kibana. elasticsearch, logstash, filebeat.

go to each directory one by one and run commands.

cd logstash
helm install logstash .

cd elasticsearch
helm install elasticsearch .

cd filebeat
helm install filebeat .

cd kibana
helm install kibana .



Create the Nginx deployment and service:

kubectl create -f nginx-deploy.yaml 
kubectl create -f nginx-svc.yaml


Create the Tomcat deployment and service:

kubectl create -f deployment.yaml
kubectl create -f svc.yaml



Create the Ingress resource:

kubectl create -f ingress.yaml



```

## Accessing the Applications

Once deployed, you can access the applications using the following URLs:

    Tomcat Application: tomcat.example.com/tomcat-app-1.0-SNAPSHOT
    Grafana: grafana.example.com
    Kibana: kibana.example.com
    Prometheus: prometheus.example.com
    Nginx Default Page: nginx.example.com

Hosts File Configuration

Make sure to update your /etc/hosts file with the following entries:

```shell
127.0.0.1 localhost
127.0.1.1 gaurav-VirtualBox

192.168.49.2 gitea.example.com 
192.168.49.2 nginx.example.com 
192.168.49.2 tomcat.example.com 
192.168.49.2 prometheus.example.com 
192.168.49.2 grafana.example.com 
192.168.49.2 kibana.example.com

```

Note: Replace 192.168.49.2 with the actual IP of your minikube instance.

Please note that some service files may have the type of service as NodePort as it was used for testing. It should be ClusterIP for use with Ingress.
