
- [commands](#commands)
- [Run a test container](#run-a-test-container)
  - [Run an NGINX container](#run-an-nginx-container)
  - [Run an container on a different port](#run-an-container-on-a-different-port)
- [Run commands inside our container](#run-commands-inside-our-container)
  - [install sudo](#install-sudo)
  - [Change the front page of nginx x](#change-the-front-page-of-nginx-x)
- [Push host-custom-static-webpage container image to Docker Hub](#push-host-custom-static-webpage-container-image-to-docker-hub)
  - [Create an image from your running container](#create-an-image-from-your-running-container)
  - [Push the image to your Docker Hub account](#push-the-image-to-your-docker-hub-account)
  - [Once you know your image runs from pulling down from Docker Hub](#once-you-know-your-image-runs-from-pulling-down-from-docker-hub)
- [Automate docker image creation using a Dockerfile](#automate-docker-image-creation-using-a-dockerfile)

**DOCKER**

# commands 

- ```docker --help``` to get help
- ``` docker ps``` to see containers running
- ``` docker stop *ID or name *``` to stop the container running 
- ```docker ps -a``` to see all containers - running or no
- ```docker start *container name/ID*``` to start up a container you've already run
- ``` docker rm *name of the container/ID*``` to remove a container (not the image)
-```docker rm -f *container name/ID*``` to force a running container to stop and remove it 
-```docker stop $(docker ps -a -q)``` stop all docker containers


# Run a test container
**may need to run as admin on bash & docker**

- use ```docker images``` to see the images you have 
- test run ``` docker run hello-world```
- it will pull from library and downloads an image and run the container
![alt text](images/1image.png)

## Run an NGINX container

``` docker run -d -p 80:80 nginx:latest```

- "-d" detached mode - runs in the background
- "-p 80:80" nginx runs on port 80, the first 80 is the first port (the port it'll run outside of the container) and the second port (the port it'll run on inside the container) is after the colon. 
- "nginx:latest" specify the tag and therefore the version you want it to pull, if there is no tag it will download the lasts

- to check go to "127.0.0.1" or "localhost" on your web browser and nginx should be running

![alt text](images/2image-1.png)

## Run an container on a different port 
- ```docker run -d -p 80:80 ahskhan/nginx-254``` if you run it - there will be an error as nginx is already running on port 80
- ```docker run -d -p 90:80 ahskhan/nginx-254``` it will now be running on port 90



# Run commands inside our container

``` docker exec -it *container id* sh```

- "exec" to execute commands 
- "-it" interact so we cna use it 
- "t" we want it running in a terminal in our container
- "sh" we are sunning s shell command 

**THIS WILL BRING A ERROR`**

- we need to set an alias
  - set one for terraform
  - ```alias tf="terraform"``` 
  - when you type "tf" it now runs the "terraform" cmd
  - this only lasts for the bash session!
- set one for the error to add winpty to the beginning of the cmd
  ```alias docker="winpty docker"``` 

## install sudo
It is a very cut down version of linux - sudo doesn't exist
- ```uname -a``` shows the name 
- ```apt-get update```
- ```apt-get upgrade```
- ```apt-get install sudo``` 

## Change the front page of nginx x
- use this path to cd to the html file  ```/usr/share/nginx/html```
- ``` apt-get install nano``` doesn't have nano -  download it 
- ```nano index.html``` nano into the html file

![alt text](3image-2.png)
![alt text](images/3image-2.png)
- CTRL + S and CTRL + X

![alt text](image-3.png)
![alt text](images/4image-3.png)
- ```exit``` to leave the container

# Push host-custom-static-webpage container image to Docker Hub

## Create an image from your running container 

1. use the command ```docker commit <container_id> <new_image_name>```
2. for this instance ```docker commit blissful_burnell```
3. you can name it later using  ```docker tag <existing_image_id> <username>/<image_name>:<tag>```

## Push the image to your Docker Hub account
use the cmd ```docker push *username*/*my_new_image*:latest```

## Once you know your image runs from pulling down from Docker Hub

```docker run -d -p 81:80 nao55/nginx-new-image:v1```

# Automate docker image creation using a Dockerfile

1. Create a new folder such as tech2xx-mod-nginx-dockerfile (not in a repo that will be published)
2. cd into the new folder on bash
3. Create an index.html you'd like to use instead of the nginx default page and nano into it and write in it

![alt text](images/newhtml.png)

1. Create a docker file ```touch Dockerfile```
2. add a script 
```
# use custom image
FROM nginx:latest

# copy the html file to the default nginx location
COPY index.html /usr/share/nginx/html/index.html
```
3. check the container exists
4. run the cmd ```docker build -t my-nginx-image . ```
   1. "-t" means the image is being tagged with the name that follows
   2. "." specifies that the build should be run in the current directory
5. push the container ```docker push nao55/nginx-automated-image```
6. Run the container ```docker run -d -p 82:80 nao55/nginx-automated-image```

![alt text](images/newnginx.png)