# Deployment of scalable and secure Django Application with Kubernetes

---

| &nbsp; | &nbsp; |
| --- | ----------- |
| **Objective** | - Deployment of scalable and secure Django Application with Kubernetes <br>- Scale up or down the number of running Pods using the replicas in Deployment manifest |
| **Approach** | - Build Kubernetes Cluster on AWS from scratch with Minikube <br>- Setup and manage Docker containers for Django and React Application into Kubernetes Pods <br>- Manage Deployment, Replication, Auto healing, Auto scaling for Kubernetes Cluster <br>- Manage Network and Services with Host IP allocation |
| **Impact** | - Reduced Downtime by 75% on Production Environments</br>- Successfully Managed Deployment, Replication, Auto healing, Auto scaling for Kubernetes Cluster |
<br>

**Primary Technology:** Kubernetes, aws EC2 service, Docker, Minikube
<br><br>


<br><br>
---


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



## 2. Setup and manage Docker containers for Django and React Application into Kubernetes Pods

### 2.1 Clone git repository
`git clone https://github.com/LondheShubham153/django-todo-cicd.git`
![1](https://user-images.githubusercontent.com/102405945/213479433-474feddd-d0c9-4444-869f-ca219aa1a13d.png)
![2](https://user-images.githubusercontent.com/102405945/213479647-d5c04c30-1760-421d-95c2-d11b98565516.png)

### 2.2 Setup and manage docker container
Build Dockerfile:
`docker build -t trainwithshubham/django-todo:latest`

Create container of the image:
`docker run -d -p 8000:8000 trainwithshubham/django-todo:latest .`

Test:
`curl -L http://52.50.75.255:8000`
![3](https://user-images.githubusercontent.com/102405945/213479724-f66c3fd7-0da2-4027-af30-50ae3581359f.png)
![4](https://user-images.githubusercontent.com/102405945/213479747-0c929657-e96e-4a1a-acbb-57b6310ebb25.png)
![5](https://user-images.githubusercontent.com/102405945/213479773-f398be59-6cde-4378-83cb-dd5804b7a072.png)

Create kubernetes pods:
`mkdir k8s`

`cd k8s`

`ls`

`docker images`

Push image on dockerhub:
`docker push trainwithshubham/django-todo`
![6](https://user-images.githubusercontent.com/102405945/213479976-e63a0a9b-35b2-46e9-b5f1-afd2cd97e6ab.png)

### 2.3 Create Kubernetes Pods using kubectl command
`vim pod.yaml`

`kubectl apply -f pod.yaml`

Test pod:
`kubectl get pods`
![7](https://user-images.githubusercontent.com/102405945/213480017-66c5bfb5-1702-4c5a-89b1-d900a566d8b8.png)
![8](https://user-images.githubusercontent.com/102405945/213480091-ee5cbefb-7808-40e8-a9e3-c2c5589c02f6.png)
![9](https://user-images.githubusercontent.com/102405945/213480171-6b064e71-4ee2-4f51-91c8-712c7973c2aa.png)



## 3. Managed Deployment, Replication, Auto healing, Auto scaling for Kubernetes Cluster

### 3.1 Create deployment file <br>
`vim deployment.yaml`

`kubectl apply -f deployment.yaml`

`kubectl get deployments`
![1](https://user-images.githubusercontent.com/102405945/213488301-56e91e93-a1e7-43a7-b598-ef0552cb24c9.png)
![2](https://user-images.githubusercontent.com/102405945/213488324-a835c944-d0cc-4843-8eee-ed7f96ac1a00.png)
![3](https://user-images.githubusercontent.com/102405945/213488339-259e3702-e01c-472f-b9c0-2cb357ebc0bd.png)

### 3.2 Test for auto healing using `kubectl get pods` command
![4](https://user-images.githubusercontent.com/102405945/213488371-5b937e11-ebce-4340-8e69-c16f7723d417.png)




## 4. Manage network and Services with Host IP allocation

### 4.1 Create service.yaml file of type NodePort
`vim service.yaml`

`kubectl apply -f service.yaml`

`kubectl get svc`

`minikube service todo-service --url`

Test: `curl -L http://192.168.49.2:30007`
![1](https://user-images.githubusercontent.com/102405945/213495271-c84cabda-67f0-4f28-9286-6cd7ac74056c.png)
<img width="788" alt="2" src="https://user-images.githubusercontent.com/102405945/213495452-57c33b03-952c-46ad-b9d6-238ad11a31ac.png">
![3](https://user-images.githubusercontent.com/102405945/213495377-2d4d3237-fe71-44f7-9544-257f254a4a5a.png)

### 4.2 Host IP allocation
`sudo vim /etc/hosts`

Add: `192.168.49.2 todo-app.com`

Test: `curl -L todo-app.com:30007`
![4](https://user-images.githubusercontent.com/102405945/213495565-29062d57-2a36-4cf4-952f-d10bacc7b3c5.png)
![5](https://user-images.githubusercontent.com/102405945/213495593-d9022094-2371-4e09-a908-a02a7fc7e2e1.png)
![6](https://user-images.githubusercontent.com/102405945/213495625-dd99e022-eda9-4457-a93c-df1ab0d7ad07.png)
![7](https://user-images.githubusercontent.com/102405945/213495636-9a6a7ca1-3ce6-4f07-ae74-2d1c20124dfb.png)
