# Docker and How to dockerize React App

### [Docker Installation](https://docs.docker.com/get-docker/)


## Terminology
- **Docker daemon**: It listens to the API requests being made through the Docker client and manages Docker Object.
- **Docker CLient**: This is what you use to interact with Docker. The client sends the commands to the daemon.
- **Docker registries**: This is where Docker images are stored.
- **Dockerfile**: Describes steps to create Docker Image.
- **Docker Image**
- **Docker Containers**



## How does it Work ?

If you run `docker run hello-world` command on your command line, it looks for the `hello-world` image in your desktop which would present in Image Cache if you have ever used `hello-world` image. If it it is not present locally on your desktop it will look for the image on the docker hub and download it. After receiving the image docker will run the instance of a container. 

An image is the blueprint to run a container or we can say A Docker image is a set of instructions that defines what should run inside a container. The Container runs only one single program.

![](https://i.imgur.com/cCgruR1.png)




## Containerization Vs VMs Virtualization
![](https://i.imgur.com/5Cmw4Lk.png)

**Containerization** is basically packaging your software code and all its necessary components and dependencies into an isolated container. It is "light-weight" and provides isolation from the host and other container. The container can be run consistently in any environment and on any infrastructure independent of that environment or infrastructure's Operating System. It helps developers to create and deploy applications faster and more securely. Multiple containers can run on the same machine and share the same os kernel with other containers. It takes less space than VMs.


**Virtual Machine(VMs)** create an abstract layer over physical Hardware .It provides complete isolation from the host operating system and other VMs. A layer called **Hypervisor** enables other operating systems to run in parallel. Hypervisor separates other VMs from one another and allocates processors, memory and storage among them. Deploying applications is not easier or faster as compare to Containers.




## Some Useful Commands.
For linux users to check whether docker is running on your system or not
```
systemctl status docker
```

### [`docker ps`](https://docs.docker.com/engine/reference/commandline/ps/)
- Shows all the the running containers
```
docker ps
```
### [`docker build`](https://docs.docker.com/engine/reference/commandline/build/)
- Build an image from `dockerfile`
```
docker build -t <image-name>:<image-tag> .
```

### [`docker run`]()
```
docker run <Imagename>/<ImageId>:(tag)<latest/verions>

i.e.

docker run -it ubuntu // same as: docker run -it ubuntu:latest 
docker run -it ubuntu:18.04
docker run -it ubuntu:16.04
```

### [`docker start`](https://docs.docker.com/engine/reference/commandline/start/)
```
docker start <Container-Name>/<Container-Id>
```
### [`docker stop`](https://docs.docker.com/engine/reference/commandline/start/)
- It stop one or more running containers
```
docker stop <Container-Name>/<ContainerId>
```

### [`docker kill`](https://docs.docker.com/engine/reference/commandline/kill/)
- It will kill one or more running container
```
docker kill <Container-Name>/<Container-Id>
```


### [`docker rm`](https://docs.docker.com/engine/reference/commandline/rm/)
- Removes the container
```shell
docker rm <Container-Name>/<Container-Id>
```
### [`docker inspect`](https://docs.docker.com/engine/reference/commandline/inspect/)
> It will only run if the image is already present in your local machine.
```
docker inspect ubuntu

in general

docker inspect <ImageName>/<ImageId>
```

### [`docker system prune`](https://docs.docker.com/engine/reference/commandline/system_prune/)
- Remove all unused containers, networks, images (both dangling and unreferenced), and optionally, volumes.
```
docker system prune
```

## What is [`docker-compose`](https://docs.docker.com/compose/) ?
Docker compose helps in running multiple container in single serivce. or we can say docker compose provides the facility to create multiple servies for your docker container. let's take an example my application require NodeJs and MySQL database. I can create one file which would start both the containers as a service without the need to start each one separately.


## Adding  `dockerfile` for your react app and run it inside a docker container.
- Create a react application make sure you have [nodejs](https://nodejs.org/en/) installed.
```
npx create-react-app myapp

cd myapp
```
- Create a dockerfile as `dockerfile` and docker ignore file `.dockerignore`

```dockerfile 
<----------------// dockerfile----------------------->

FROM node                👈 you can add any version you required

WORKDIR "/app"           👈 inside your docker container at /app 
                            directory installs all your dependencies 
COPY package.json /app/ 

RUN npm install   

COPY . /app/             👈 to copy the files into your docker container

CMD ["npm", "start"]     👈 to run the server at port :3000

```
![](https://i.imgur.com/QlAV69f.png)


Inside your `.dockerignore` file
```
<----------------// .dockerignore----------------------->


node_modules
```

- After creating `dockerfile`, we need to create docker image with the help for `dockerfile`.
```
docker build --tag <image-name>:<image-tag> .   👈 it's upto you whether you
                                                   want to provide image-tag 
                                                   or not by default it will 
                                                   be (latest)
```

```
docker run <image-name>:<image-tag>

docker ps                   // you can check your running container
```

To stop the container and remove the image
```
docker stop <Container-Id>

docker rmi <Image-Name>:<Image-tag>

```

## Resources 
- https://docs.docker.com/