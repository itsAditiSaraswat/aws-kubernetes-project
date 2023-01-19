## 1. Build Kubernetes Cluster on AWS from scratch with Minikube

### 1.1 Launch EC2 instance on AWS 
Configure the server to ubuntu and select Amazon Machine Image (AMI) to t2.medium since we need two CPUs for minikube
![1](https://user-images.githubusercontent.com/102405945/213396420-624736cb-2157-4d79-b67b-f0222631511a.png)
![2](https://user-images.githubusercontent.com/102405945/213396586-e36f2a32-3f98-42ea-8fe1-f43ce9bf6368.png)
![3](https://user-images.githubusercontent.com/102405945/213396605-0567c58c-7983-4aa5-b10e-0eee3a7b49a9.png)


### 1.2 Connect Instance
![4](https://user-images.githubusercontent.com/102405945/213396632-ab831e30-98f6-4c13-9bc9-a37ce028bfec.png)


### 1.3 Install the latest minikube stable release on x86-64 Linux using binary download
`curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64` <br>

`sudo install minikube-linux-amd64 /usr/local/bin/minikube`
![5](https://user-images.githubusercontent.com/102405945/213396886-a1441f33-ddca-400a-bba2-e9b7c631d03f.png)


### 1.4 Install docker Engine
**Install using the repository** <br>

Before you install Docker Engine for the first time on a new host machine, you need to set up the Docker repository. Afterward, you can install and update Docker from the repository.

**Set up the repository**
- Update the `apt` package index and install packages to allow `apt` to use a repository over HTTPS:   
    `$ sudo apt-get update` <br>
    
    `$ sudo apt-get install \
        ca-certificates \
        curl \
        gnupg \
        lsb-release`
        
- Add Docker’s official GPG key:   
    `$ sudo mkdir -p /etc/apt/keyrings` <br>
    
    `$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg`
    
- Use the following command to set up the repository:
    `$ echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null`
![6](https://user-images.githubusercontent.com/102405945/213397126-e577dacd-6edc-4bed-9086-ec17ffb5da90.png)
![7](https://user-images.githubusercontent.com/102405945/213397143-55fd3098-c348-4b59-a010-0e435a54faf7.png)


**Install Docker Engine**
- Update the `apt` package index:    
    `$ sudo apt-get update`
    
- Install Docker Engine, containerd, and Docker Compose.<br>
    To install the latest version, run:   
     `$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin`
    
    
### 1.5 Start cluster
`sudo usermod -aG docker $USER && newgrp docker`

`minikube start --driver=docker`
![8](https://user-images.githubusercontent.com/102405945/213397360-2e767004-af77-49c2-9af2-3f6231a4f08f.png)


### 1.6 Install kubectl
`sudo snap install kubectl --classic`

`kubectl get po -A`
![9](https://user-images.githubusercontent.com/102405945/213397414-34f1831e-6e46-4baf-8131-39c46c29801c.png)



## 2. Setup and managed Docker containers for Django and React Application into Kubernetes Pods

### 2.1 Clone git repository
`git clone https://github.com/LondheShubham153/django-todo-cicd.git`

### 2.2 Setup and manage docker container
`docker build -t trainwithshubham/django-todo:latest`

`docker run -d -p 8000:8000 trainwithshubham/django-todo:latest`

`mkdir k8s`

`cd k8s`

`ls`

`docker images`

`docker push trainwithshubham/django-todo`

### 2.3 Create Kubernetes Pods using kubectl command
`vim pod.yaml`

`kubectl apply -f pod.yaml`

`kubectl get pods`
