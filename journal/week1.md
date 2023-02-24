# Week 1 — App Containerization
## Dockerfile
We check with the followings commands that Python is running
```
cd backend-flask
export FRONTEND_URL="*"
export BACKEND_URL="*"
python3 -m flask run --host=0.0.0.0 --port=4567
cd ..
```
> **Note**
> We have to unlock the port 4567

### Add a Dockerfile backend, frontend and a docker-compose.yml
Link to the commits: https://github.com/BeluPuentes/aws-bootcamp-cruddur-2023/commit/fc46d702f7513321591a971c539f2a5e3d8355ac  

Then we build the container 
```
docker build -t backend-flask ./backend-flask
```
And run the container 
```
docker container run --rm -p 4567:4567 -d backend-flask
```
Return the container id into an Env Vat
```
CONTAINER_ID=$(docker run --rm -p 4567:4567 -d backend-flask) env | grep CONTAINER
```
Get Container Images or Running Container IDs
```
docker ps
docker images
```

## Add Notifications page
Here is the link to the commit: https://github.com/BeluPuentes/aws-bootcamp-cruddur-2023/commit/2f8f5c8b7ce89bca17802c404fe692cb7703ab4a

## DynamoDB Local and PostgreSQL
We can see in the gitpod.yml that PostgeSQL is installed 
Link to commit: https://github.com/BeluPuentes/aws-bootcamp-cruddur-2023/commit/25f433f7015df8d51ece7a708fbc6f0969ecea6d
![image](https://user-images.githubusercontent.com/93335543/221254569-9a99bc51-5fa5-4be8-9671-33d7510d9489.png)

Connecting via AWS-CLI
```
sudo apt-get install -y postgresql-client
psql -h localhost -U postgres
>\l
```
Output
![image](https://user-images.githubusercontent.com/93335543/221255504-ccb10a05-75e4-4887-8cfb-25e089f5b0ea.png)

## Homework Challenges
### 1. Run the dockerfile CMD as an external script
First we need to create a new file in the backend, called `flask.sh`
Link to see what the file contains: https://github.com/BeluPuentes/aws-bootcamp-cruddur-2023/blob/main/backend-flask/flask.sh  
Then we build the container 
```
docker build -t backend-flask ./backend-flask
```
And run the container
```
docker run -d --rm -p 4567:4567 -it -e FRONTEND_URL='*' -e BACKEND_URL='*' backend-flask
```

### 2. Push and tag a image to DockerHub 
In order to do this, we need to create an account in https://hub.docker.com/
Then we log into the docker hub, with the follwing command
```
docker login
```
Output 
```
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: 
Password: 
WARNING! Your password will be stored unencrypted in /home/gitpod/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
```
There is a warning that remains us to log out, when we finish working 

The first thing we have to do, is to tag our image 
```
docker tag backend-flask belenpuentes/cruddur-backend-flask:1.0
```
Then we push the image 
```
docker push belenpuentes/cruddur-backend-flask:1.0
```
When it finished pushing, we enter in our dockerhub and we can see our image 
![image](https://user-images.githubusercontent.com/93335543/221257649-f88509e0-6f90-4b0b-8d23-7a5e51af6d01.png)

Remember to logout withe the following command
```
docker logout
```

### 3. Use multi-stage building for a Dockerfile build
It is use to build out an application with the minimum that it requiers in order to funcionate correctly. This is the reason why, we can see a reduction in the size of the image
Here is the documentation of Multi-Stage: https://docs.docker.com/build/building/multi-stage/  
So before we do this, let me show you my original size of the image 
```
docker build images backend-flask:latest
```
![image](https://user-images.githubusercontent.com/93335543/221262465-f1cc5dd5-329d-4655-9b3d-f7da1ed94f2a.png)

Adding the Multi-stage in d=the Dockerfile 
https://github.com/BeluPuentes/aws-bootcamp-cruddur-2023/commit/83418a2bba039aed07c990e87e09c8705ca8b3eb?diff=unified

We re-build the container in order to size the diffeence 
```
docker build -t backend-flask ./backend-flask
docker build images backend-flask:latest
```
![image](https://user-images.githubusercontent.com/93335543/221263861-f3aa184d-3f3d-4131-8c3d-1fa10aa2deb1.png)
It works propertly 


### 4. Implement a healthcheck in the V3 Docker compose file
We need to add the health check in the docker-compose.yml
Link to the commit: https://github.com/BeluPuentes/aws-bootcamp-cruddur-2023/commit/26faa785c0237769544cb1e0080c70726c19391a  
```
# Healthchek:
    healthcheck:
      test: curl --fail https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}  
      interval: 60s
      timeout: 3s
      start_period: 5s
      retries: 3
```
In order to make this work, we need to add the followigs commands in the DockerFile 
```
RUN apt-get update 
RUN apt-get install -y gcc
RUN apt-get install -y curl
```
> **note**
> If we use wget we don't need to add the commands above 
> ```
> healthcheck:
>   test: wget --no-verbose --tries=1 --spider https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST || exit 1
>   interval: 60s
>   retries: 5
>   start_period: 20s
>   timeout: 10s

The result is 
![image](https://user-images.githubusercontent.com/93335543/221272281-735bb5e6-e31c-44d7-b186-32cf12e6e083.png)
In case something goes wrong it will appear **unhealthy**

### 5. Research best practices of Dockerfiles and attempt to implement it in your Dockerfile

### 6. Learn how to install Docker on your localmachine and get the same containers running outside of Gitpod / Codespaces
I open my VSCode
In the terminal I launch the following command
```
git clone https://github.com/BeluPuentes/aws-bootcamp-cruddur-2023.git
```
After that it started downloading the repository
![image](https://user-images.githubusercontent.com/93335543/221278104-90a92e2a-bcd7-418e-acfa-a9b309ac1719.png)
When it finished I just write
```
docker-compose up
```
With that my containers were up 
![image](https://user-images.githubusercontent.com/93335543/221303886-347eaca6-8b56-4d31-a473-34fc2ea69887.png)


### 7. Launch an EC2 instance that has docker installed, and pull a container to demonstrate you can run your own docker processes. 
We need to go to AWS Console, the to the part of EC2 and launch an instance 
![image](https://user-images.githubusercontent.com/93335543/221304930-679a5a83-7c19-41c5-8133-ebd1a9481db7.png)
A file `.pem` is downloaded, with Puttygen, we generate a new file `.ppk`
We need that, ther user name of the instance and the public DNS in order to connect 
![image](https://user-images.githubusercontent.com/93335543/221321044-9e82fd0a-f13f-4fb5-96bd-ca3f09a6f1c9.png)
We use the followings commands to install Docker 
```
sudo yum update -y
sudo amazon-linux-extras install docker -y
sudo service docker start
sudo usermod -a -G docker ec2-user
```
Then we launch a container with thw following command
```
sudo docker pull nginx
```
![image](https://user-images.githubusercontent.com/93335543/221321336-3e2e584d-24bb-4315-8b9f-3af444cbe107.png)
