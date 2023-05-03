# Install Docker Compose and MiniKube

On the Ansible Control Host run the following commands

```
#Update and Upgrade
sudo apt update
sudo apt upgrade

#Install Docker
sudo apt install docker-compose

#Test docker
sudo docker run hello-world

#Test Docker Compose
docker-compose -version

sudo -i
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -

exit

#Install curl and virtual box
sudo apt install curl
sudo apt install virtualbox virtualbox-ext-pack

#Minikube requires kubectl to manage Kubernetes clusters. Install kubectl with the following command:
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
chmod +x kubectl
sudo mv kubectl /usr/local/bin/

#Download and install Minikube:
#You can download Minikube using curl:
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```
Start Minikube:
Start Minikube with the following command:
```
minikube start
```


This may take a few minutes to complete. Once it finishes, you can check the status of your Minikube cluster with:
```
minikube status

```

Use Minikube:
You can now use Minikube to run Kubernetes applications. For example, you can create a sample deployment:

```
kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.4
```

And then expose it as a service:
```
kubectl expose deployment hello-minikube --type=NodePort --port=8080
```

You can then access the service by running:
```
minikube service hello-minikube
```

This will open a web browser to the exposed service.

That's it! You now have Minikube installed on your Ubuntu system and can start experimenting with Kubernetes.





