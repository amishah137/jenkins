# Jenkins Tutorial

## Requirements
* Sample flask application.
* Dockerfile to build the image
* Jenkins pipeline for CI/CD Automation
* Dockerhub Account
* Ubuntu with Jenkins and Docker installed on it

Create a new conda environment named jenkins and activate the environment.
```
conda create --name jenkins python
conda activate jenkins
```

Check whether docker is installed using below command. if not, then install the Docker.
```
docker --version
```

Check whether jenkins is installed using below command. if not, then install the jenkins.
```
systemctl status jenkins
```

1. Open the jenkins Dashboard and select new item.

![image](https://github.com/amishah137/jenkins/assets/11003645/8fe36570-7bfa-421f-bc10-58a0a2ac937d)

2. Type name 'docker-flaskapp', select pipeline and press 'OK'

![image](https://github.com/amishah137/jenkins/assets/11003645/9c71a469-168b-4aa0-b193-1435475e844b)

3. The new window will popup. Write description "This is my first demo"

![image](https://github.com/amishah137/jenkins/assets/11003645/371b128c-eaa4-4496-9dbf-6d91ac100162)

4. Scroll down in the window and choose 'advanced'

![image](https://github.com/amishah137/jenkins/assets/11003645/c5cd7835-ae6c-489b-83d4-df2578a490b0)

5.  Fill the Display name 

![image](https://github.com/amishah137/jenkins/assets/11003645/13f9e3b6-1770-4f53-9d6a-1b336020bc53)

6. Fill the pipeline information as below and press "save":

![image](https://github.com/amishah137/jenkins/assets/11003645/c75b0fdd-68b3-477f-aecb-a7f62508f7b1)

7. Press "Build now"

![image](https://github.com/amishah137/jenkins/assets/11003645/6b412aae-fcd0-4f0c-a180-5f2043b6a5cd)

8. The execution is successful.

![image](https://github.com/amishah137/jenkins/assets/11003645/a61da11e-7dab-4807-9a19-8bba367bf787)


9. 
10.  


### Let's look at the contents of Dockerfile.
```
FROM python:3.6
RUN pip install flask
COPY . /opt/
EXPOSE 8080
WORKDIR /opt
ENTRYPOINT ["python", "app.py"]
```
The Dockerfile outlines the instructions for building a Docker image. Each line serves a specific purpose:
* FROM python:3.6: This line specifies the base image for the Docker image. In this case, it uses the official Python 3.6 image as the starting point.
* RUN pip install flask: This line executes a command during the image build process. It uses the pip package manager to install the Flask framework, which is a popular web framework for Python.
* COPY . /opt/: This line copies the contents of the current directory (where the Dockerfile resides) to the /opt/ directory within the Docker image. It ensures that all the necessary files, including the source code, are available in the image.
* EXPOSE 8080: This line informs Docker that the container, based on this image, will listen on port 8080. It does not actually publish the port to the host machine, but it serves as a documentation for other developers.
* WORKDIR /opt: This line sets the working directory inside the Docker container to /opt/. It means that subsequent commands will be executed from this directory.
* ENTRYPOINT ["python", "app.py"]: This line specifies the default command to be executed when a container is run from the image. It uses the python interpreter to run the app.py file. This assumes that the app.py file is present in the working directory, which is set to /opt/ earlier.

To summarize, this Dockerfile sets up a Docker image based on Python 3.6, installs Flask, copies the current directory's contents to the /opt/ directory inside the image, exposes port 8080 for future use, sets the working directory to /opt/, and configures the container to run the app.py file as its entry point.

### Let's look at the contents of Jenkinsfile.
```
pipeline {
    agent any 
    environment {
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
    }
    stages { 

        stage('Build docker image') {
            steps {  
                sh 'docker build -t amishah137/flaskapp:$BUILD_NUMBER .'
            }
        }
        stage('login to dockerhub') {
            steps{
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('push image') {
            steps{
                sh 'docker push amishah137/flaskapp:$BUILD_NUMBER'
            }
        }
}
post {
        always {
            sh 'docker logout'
        }
    }
}
```
The Jenkins file describes a Jenkins pipeline that automates the build and deployment of a Docker image for a Flask application. Let's go through its contents step by step:

1. pipeline: This keyword indicates the start of the pipeline block, which defines the entire Jenkins pipeline.
2. agent any: This line specifies that the pipeline can run on any available agent (Jenkins slave). The pipeline will be executed on any available node in the Jenkins environment.
3. environment: This block defines environment variables used within the pipeline. 
* DOCKERHUB_CREDENTIALS = credentials('dockerhub'): This line assigns the value of the 'dockerhub' credentials to the DOCKERHUB_CREDENTIALS environment variable. The 'dockerhub' credentials are assumed to be defined and stored securely in Jenkins.
4. stages: This block defines the different stages of the pipeline. Stages represent logical divisions of the build and deployment process.
* stage('Build docker image'): This stage is responsible for building the Docker image of the Flask application.
    * steps: This block contains the steps to be executed within the stage.
    * sh 'docker build -t amishah137/flaskapp:$BUILD_NUMBER .': This line executes a shell command to build a Docker image using the docker build command. The -t flag specifies the image name with the format amishah137/flaskapp:$BUILD_NUMBER, where $BUILD_NUMBER is a Jenkins environment variable that represents the current build number.
* stage('login to dockerhub'): This stage logs in to DockerHub.
    * steps: This block contains the steps to be executed within the stage.
    * sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin': This line uses the stored DockerHub credentials to log in to DockerHub using the docker login command. The username is retrieved from the DOCKERHUB_CREDENTIALS_USR environment variable, and the password is retrieved from the DOCKERHUB_CREDENTIALS_PSW environment variable. 
* stage('push image'): This stage pushes the Docker image to DockerHub.
    * steps: This block contains the steps to be executed within the stage.
    * sh 'docker push amishah137/flaskapp:$BUILD_NUMBER': This line pushes the previously built Docker image to DockerHub using the docker push command.
15. post: This block defines post-build actions to be executed after all stages are completed. In this case, it includes a single step that always executes, regardless of the pipeline outcome.
* always: This keyword specifies that the step should always be executed.
    * sh 'docker logout': This line logs out from DockerHub using the docker logout command.
 
Overall, this Jenkins file sets up a pipeline that builds a Docker image, logs in to DockerHub using provided credentials, pushes the image to DockerHub, and finally logs out.
