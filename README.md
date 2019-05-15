# WordPress Deployment Guide

This test is to demo complete CI and CD of the WordPress CMS.
This application does not use pre-built WordPress docker images.
### Requirements
To demo the application git access to git repo is needed. To demonstrate the CICD Jenkins is used. To store the docker images public docker registry is used.

**Required tools**

Git/Github
Jenkins
Docker
Kubernetes

### Approach

We will set up continuous delivery pipelines for containerized (Docker) application to Kubernetes. Through this approach plan to deliver the infrastructure as a code. The backed database is MySQL.

1. **Pipeline as code**: We use Jenkins file to create declarative pipelines
2. **Infrastructure as code**: We containerize the application using Docker
3. **Deployment as code**: We use Kubernetes as our container orchestrator. Our deployments are declarative via code.

Code developed in the laptop is checkout to SCM, in this case, GitHub after the code is committed. Once the code is approved it is merged to the master branch. Based on the requirement build will be triggered in Jenkins when the new code is detected or build is run based on the schedule.
Jenkins does the below steps.
1. clones the new code.
2. **Build the docker image** :
Pulls the base docker images needed to build the application and builds the image using the Dockerfile that is available in the working directory. Once the image is successfully built it will then upload this image to the docker registry that is provided.
3. **Testing the code**: 
Static code analysis, Unit testing, integration testing is done in this step
4. **Clean the old images**
This step cleans the unnecessary images and files on the server.
5. Deploy Application (Future Enhancement)
This step will enhance the functionality by having the option to deploy to different environments.
6. Wait for services to be up or for 120 retries.
This step will wait for the services to be up and accessible. This will wait for 120 retries.

### How to Run

First, we need to have a Git repo URL, in this case: https://github.com/nagaraj1171/WordPress-CI-CD-K8.git

Second, we need to create the pipeline in Jenkins.
1. Go to New Item
2. Enter a new item name
3. Select 'Pipeline' and click OK
4. Give a description for the project in General then, tick mark "GitHub project" and then provide Project URL i.e GitHub URL.
5. Select "This project is parameterized" and add in 3 parameters and choose the parameter type accordingly.    
    1. DOCKER_USERNAME (This is a docker registry username, type string)
    2. DOCKER_PASSWORD (This is to key in docker registry password, type password)
    3. namespace_gui (This is for future enhancements to configure stage, development and production deployments, type choice)
    4. mysql_password (This is to key in MySQL password)
6. You can choose the appropriate options in Build Triggers based on the need.
7. Last but not the least, Pipeline section here we need to select Pipeline script from SCM. Select Git as SCM and provide the repository URL and details.
   Ensure the script path to be Jenkinsfile
8. Save the configuration. and Build the project by Build with Parameters.

### How to verify

Successful completion of the build in Jenkins the result has the URL that can be used to access the WordPress application.