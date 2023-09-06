---
title: "Deployed a Reddit Copy on Kubernetes with Ingress Enabled"
datePublished: Wed Sep 06 2023 08:23:03 GMT+0000 (Coordinated Universal Time)
cuid: clm7h2cdv00070ak0f1f31zg0
slug: deployed-a-reddit-copy-on-kubernetes-with-ingress-enabled

---

**Prerequisite:** First You have to install some Prerequisites for this deployment

prerequisites installed:

1. EC2 ( AMI- Ubuntu, Type- t2.medium )
    
2. Docker
    
3. Minikube
    
4. kubectl
    
5. ```plaintext
     # Steps:-
     
     # For Docker Installation
     sudo apt-get update
     sudo apt-get install docker.io -y
     sudo usermod -aG docker $USER && newgrp docker
     
     # For Minikube & Kubectl
     curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
     sudo install minikube-linux-amd64 /usr/local/bin/minikube 
     
     sudo snap install kubectl --classic
     minikube start --driver=docker
    ```
    

You can Install all this by doing the below steps one by one. and these steps are for Ubuntu AMI.

# **Step 1: Clone the source code**

The first step is to clone the source code for the app. You can do this by using this command :- git clone [https://github.com/pratik596/reddit-clone-k8s-ingress.git](https://github.com/pratik596/reddit-clone-k8s-ingress.git)

# **Step 2: Containerize the Application using Docker**

* Write a Dockerfile with the following code:
    
* ```plaintext
      FROM node:19-alpine3.15
      
      WORKDIR /reddit-clone
      
      COPY . /reddit-clone
      RUN npm install
      
      EXPOSE 3000
      CMD ["npm","run","dev"]
    ```
    

# **Step 3) Building Docker-Image**

Now it's time to build Docker Image from this Dockerfile. `docker build -t <DockerHub_Username>/<Imagename> .` use this command to build a docker image after Check with docker ps

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693984769301/437e5c80-943a-47c6-b5ea-0428f51f6ac4.jpeg align="center")

# **Step 4) Push the Image To DockerHub**

Now push this Docker Image to DockerHub so our Deployment file can pull this image & run the app in Kubernetes pods.

* First login to your DockerHub account using Command i.e `docker login` and give your username & password.
    
* Then use `docker push <DockerHub_Username>/<Imagename>` for pushing to the DockerHub.
    
* You can use an existing docker image i.e ***pratik596/reddit-clone*** for deployment.
    
* ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693984957086/1900e910-3fce-4b1d-84d0-48e7e979f118.jpeg align="center")
    

# **Step 5) Write a Kubernetes Manifest File**

Now you create below file in reddit-clone-k8s-ingress Folder

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693985138292/9a048db3-c3b8-4495-a2b2-69cc027734f0.jpeg align="center")

1. ## **Write Deployment.yml file**
    

Let's Create a Deployment File For our Application. Use the following code for the ***Deployment.yml*** file.

```plaintext
apiVersion: apps/v1
kind: Deployment
metadata:
  name: reddit-clone-deployment
  labels:
    app: reddit-clone
spec:
  replicas: 2
  selector:
    matchLabels:
      app: reddit-clone
  template:
    metadata:
      labels:
        app: reddit-clone
    spec:
      containers:
      - name: reddit-clone
        image: pratik596/reddit-clone
        ports:
        - containerPort: 3000
```

## **2.Write Service.yml file**

```plaintext
apiVersion: v1
# Indicates this as a service
kind: Service
metadata:
  # Service name
  name: reddit-clone-service
  labels:
    app: reddit-clone
spec:
  type: NodePort

  ports:
    # Port Map
  - port: 3000
    targetPort: 3000
    nodePort: 31000
  selector:

    app: reddit-clone
```

# **Step 5) Deploy the app to Kubernetes & Create a Service For It**

1\. kubectl apply -f Deployment.yml

1. kubectl apply -f Service.yml
    
2. See the output Check both running Command :- kubectl get deployment & kubectl get service
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693987531740/275fb506-4f6d-4fdf-8ab3-f24c9fda490c.jpeg align="center")
    

# **Step:-6) Configure Ingress**

Write ingress.yml and put the following code in it:

```plaintext
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-reddit-app
spec:
  rules:
  - host: "domain.com"
    http:
      paths:
      - pathType: Prefix
        path: "/test"
        backend:
          service:
            name: reddit-clone-service
            port:
              number: 3000
  - host: "*.domain.com"
    http:
      paths:
      - pathType: Prefix
        path: "/test"
        backend:
          service:
            name: reddit-clone-service
            port:
              number: 3000
```

Minikube doesn't enable ingress by default; we have to enable it first using `minikube addons enable ingress` command.

* If you want to check the current setting for addons in minikube use `minikube addons list` command.
    

Now you can able to create ingress for your service. `kubectl apply -f ingress.yml` use this command to apply ingress settings.

* Verify that the ingress resource is running correctly by using `kubectl get ingress ingress-reddit-app` command.
    

# **Step 8) Expose the app**

1. First We need to expose our deployment so use `kubectl expose deployment reddit-clone-deployment --type=NodePort` command.
    
2. You can test your deployment using curl -L [**http://172.31.81.212:31000**](http://192.168.49.2:31000). **172.31.81.212** is a minikube ip & Port **31000** is defined in Service.yml
    
3. Then We have to expose our app service `kubectl port-forward svc/reddit-clone-service 3000:3000 --address 0.0.0.0 &`
    
    see this output after curl run :-
    
4. ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693988223382/1cb94ff5-246d-4366-8d00-2754f9fd9e4f.jpeg align="center")
    

Now you can Add a port 3000 in aws security group .

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693988976466/38d9451d-1662-48c1-8e7e-cac1d891f256.jpeg align="center")

You can also see the deployed application on see the output **Ec2\_ip:3000**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693988523139/69b9a16e-0e2e-4e79-873c-98a08e8bc970.jpeg align="center")