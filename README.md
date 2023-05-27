# Jenkins Tutorial

## Requirements
Sample flask application.
Dockerfile to build the image
Jenkins pipeline for CI/CD Automation
Dockerhub Account
Ubuntu with Jenkins and Docker installed on it

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

1. Open the jenkins and select new item.
[Screenshot from 2023-05-27 19-30-27](https://github.com/amishah137/jenkins/assets/11003645/7abeb602-4602-42c6-91ee-6dbc3a62cd7a)
2. 
3. Select pipeline
4. Provide Jenkinsfile
5. Press 'Build now'!


