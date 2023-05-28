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

