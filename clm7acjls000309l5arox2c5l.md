---
title: "Jenkins CI/CD Pipeline for a Node.js App with Docker, GitHub, Docker Hub on AWS EC2"
datePublished: Wed Sep 06 2023 05:15:02 GMT+0000 (Coordinated Universal Time)
cuid: clm7acjls000309l5arox2c5l
slug: jenkins-cicd-pipeline-for-a-nodejs-app-with-docker-github-docker-hub-on-aws-ec2

---

In this tutorial, guide you through the process of setting up a Jenkins CI/CD pipeline for a Node.js application using Docker, GitHub, Docker Hub, and an AWS EC2 instance. This pipeline will automate the build and deployment process, allowing you to easily manage and deploy your Node.js app.

## **Prerequisites**

Before we begin, ensure you have the following prerequisites in place:

1. **Node.js App on GitHub:** Your Node.js application code should be hosted on GitHub. If it's not already there, create a repository and push your code to it.
    
2. **Docker Hub Account:** You will need an account on Docker Hub to store your Docker images.
    
3. **AWS EC2 Instance:** Set up an AWS EC2 instance with the necessary permissions and configurations. Ensure that Jenkins is installed and running on this instance.
    

## **Step 1: Install Jenkins Plugins**

1. Log in to your Jenkins dashboard.
    

1. Click on "Manage Jenkins" on the left sidebar.
    
2. Navigate to "Manage Plugins."
    
3. Go to the "Available" tab, and search for and install the following plugins:
    
    * GitHub Integration
        
    * Docker
        
    * Pipeline
        

## **Step 2: Configure Jenkins Global Credentials**

1. In the Jenkins dashboard, go to "Manage Jenkins" &gt; "Manage Credentials."
    
2. Click on "Global credentials (unrestricted)."
    
3. Add your Docker Hub credentials as "Username with Password" credentials.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693976522394/37724351-504c-4754-88cd-b6837e187e9d.jpeg align="center")
    

## **Step 3: Create a Jenkins Pipeline Job**

1. Click on "New Item" on the Jenkins dashboard.
    
2. Enter a name for your pipeline job and select "Pipeline" as the project type.
    
3. In the pipeline configuration, under "Pipeline," choose "Pipeline script "
    
    ```plaintext
    pipeline {
        agent any
        
        stages{
            stage("Code"){
                steps{
                    git url: "https://github.com/pratik596/node-todo-cicd.git", branch: "master"
                }
            }
            stage("Build & Test"){
                steps{
                    sh "docker build . -t node-app-test-new"
                }
            }
            stage("Push to DockerHub"){
                steps{
                    withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                        sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                        sh "docker tag node-app-test-new ${env.dockerHubUser}/node-app-test-new:latest"
                        sh "docker push ${env.dockerHubUser}/node-app-test-new:latest" 
                    }
                }
            }
            stage("Deploy"){
                steps{
                    sh "docker-compose down && docker-compose up -d"
                }
            }
        }
    }
    ```
    
4. Select "Git" as the SCM and provide the URL of your GitHub repository.
    
5. Under "Credentials," select the appropriate GitHub credentials.
    
6. In the "Script Path" field, specify the path to your Jenkinsfile (e.g., `Jenkinsfile` if it's in the root of your GitHub repository).
    
7. Save your pipeline job.
    

## **Step 4: Create a Jenkinsfile**

A Jenkinsfile defines your pipeline stages and actions. Here's a simple example for a Node.js app with Docker:

Replace `your-dockerhub-username` with your Docker Hub username and configure Jenkins credentials for Docker Hub.

## **Step 5: Run the Jenkins Pipeline**

1. Go to your Jenkins dashboard and click on your pipeline job.
    
2. Click on "Build Now" to manually trigger the pipeline.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693977025628/f8060377-f5cb-44a7-94c8-0cef9ee87c3e.jpeg align="center")
    
3. Jenkins will clone your GitHub repository, build a Docker image, and push it to Docker Hub.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693977109870/0e4e1fe7-4fa7-4c0c-99e6-163f409228e9.jpeg align="center")
    

## **Conclusion**

You have successfully set up a Jenkins CI/CD pipeline for your Node.js application, automating the build and deployment process using Docker, GitHub, Docker Hub, and an AWS EC2 instance. This pipeline will streamline your development workflow and ensure consistent and reliable deployments of your Node.js app. You can further extend it to include deployment to your target environment.