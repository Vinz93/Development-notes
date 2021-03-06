# Docker

## Concepts

### Image
An **image** is a lightweight, stand-alone, **executable package** that **includes everything needed to run** a piece of software, including the code, a runtime, libraries, environment variables, and config files.

### Container
A container is a **runtime instance of an image**—what the image becomes in memory when actually executed. It runs completely **isolated** from the host environment by default, only accessing host files and ports if configured to do so.

Containers run apps natively on the host machine’s kernel. They have *better **performance** characteristics than virtual machines* that only get virtual access to host resources through a hypervisor. Containers can get native access, each one running in a discrete process, taking no more memory than any other executable.
## Dockerfile

`Dockerfile` will define what goes on in the environment inside your container. **Access** to resources like **networking interfaces** and disk drives is virtualized inside this environment, which is isolated from the rest of your system, so you have to **map ports to the outside world**, and be specific about what files you want to “copy in” to that environment. However, after doing that, you can expect that the build of your app defined in this Dockerfile will behave exactly the same wherever it runs.

Eg. Dockerfile
```
# Use an official Python runtime as a parent image
FROM python:2.7-slim

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
ADD . /app

# Install any needed packages specified in requirements.txt
RUN pip install -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```

## Create images
You are going to create the image with the `Dockerfile`.

```
$ docker build -t imagetag .
```
This creates a Docker image, which we’re going to tag using `-t` so it has a friendly name.
```
$ docker images
```
The command above list your images.


## Run the app

Run the app, mapping your machine’s port 4000 to the container’s published port 80 using `-p`.

```
$ docker run -p 4000:80 imagetag
```

you can run the app in background with the option `-d`.

```
$ docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED
1fa4ab2cf395        friendlyhello       "python app.py"     28 seconds ago
```

`$ docker container stop` to end the process, using the `CONTAINER ID`, like so
```
$ docker container stop 1fa4ab2cf395
```

## Share your image
### Push your images to Docker hub
```
$ docker login
$ docker tag imagename username/repository:tag
$ docker push username/repository:tag
```
### Pull and run the image from the remote repository
From now on, you can use docker run and run your app on any machine with this command:
```
$ docker run -p 4000:80 username/repository:tag
```
No matter where `docker run` executes, it pulls your image, along with all the dependencies of your app, and runs your code. It all travels together in a neat little package, and the host machine doesn’t have to install anything but Docker to run it.

### Basic Docker commands

```
docker build -t friendlyname .  # Create image using this directory's Dockerfile
docker run -p 4000:80 friendlyname  # Run "friendlyname" mapping port 4000 to 80
docker run -d -p 4000:80 friendlyname         # Same thing, but in detached mode
docker container ls                                # List all running containers
docker container ls -a             # List all containers, even those not running
docker container stop <hash>           # Gracefully stop the specified container
docker container kill <hash>         # Force shutdown of the specified container
docker container rm <hash>        # Remove specified container from this machine
docker container rm $(docker container ls -a -q)         # Remove all containers
docker image ls -a                             # List all images on this machine
docker image rm <image id>            # Remove specified image from this machine
docker image rm $(docker image ls -a -q)   # Remove all images from this machine
docker login             # Log in this CLI session using your Docker credentials
docker tag <image> username/repository:tag  # Tag <image> for upload to registry
docker push username/repository:tag            # Upload tagged image to registry
docker run username/repository:tag                   # Run image from a registry
```


## Services

In a distributed application, different pieces of the app are called “services.” For example, if you imagine a video sharing site, it probably includes a service for storing application data in a database, a service for video transcoding in the background after a user uploads something, a service for the front-end, and so on.

Services are really just “*containers in production.*” **A service only runs one image**, but it codifies the way that image runs—what ports it should use, how many **replicas** of the container should run so the service has the capacity it needs, and so on. **Scaling a service changes the number of container instances running that piece of software**, assigning more computing resources to the service in the process.

A `docker-compose.yml` file is a YAML file that defines how Docker containers should behave in production.

E.g *docker-compose.yml*
```
version: "3"
services:
  web:
    # replace username/repo:tag with your name and image details
    image: username/repo:tag
    deploy:
      replicas: 5
      resources:
        limits:
          cpus: "0.1"
          memory: 50M
      restart_policy:
        condition: on-failure
    ports:
      - "80:80"
    networks:
      - webnet
networks:
  webnet:
```  

The file above tells Docker to do the following actions

- Pull the image.

- Run **5 instances of that image** as a service called *web*, limiting each one to use, at most, 10% of the CPU (across all cores), and 50MB of RAM.

- Immediately restart containers if one fails.

- Map port 80 on the host to web’s port 80.

- Instruct web’s containers to share port 80 via a load-balanced network called webnet. (Internally, the containers themselves will publish to web’s port 80 at an ephemeral port.)

- Define the webnet network with the default settings (which is a load-balanced overlay network).

### Run your new load-balanced app

Run your app with the following commands and your `docker-compose.yml`.
just in case of problems
``--listen-addr $(docker-machine ip node1):2377`

```
$ docker swarm init --advertise-addr 127.0.0.1
$ docker stack deploy -c docker-compose.yml getstartedlab
```

You can check your service and each container
```
$ docker service ls
$ docker service ps <service>
docker inspect --format='{{.Status.ContainerStatus.ContainerID}}' <task>
```

### Take down the app and the swarm

Take down the app
```
$ docker stack rm getstartedlab
```

Take down the swarm
```
$ docker swarm leave --force
```


### Basic commands for services
```
docker stack ls                                            # List stacks or apps
docker stack deploy -c <composefile> <appname>  # Run the specified Compose file
docker service ls                 # List running services associated with an app
docker service ps <service>                  # List tasks associated with an app
docker inspect <task or container>                   # Inspect task or container
docker container ls -q                                      # List container IDs
docker stack rm <appname>                             # Tear down an application
docker swarm leave --force      # Take down a single node swarm from the manager
```

## Understanding Swarm clusters

A swarm is a group of machines that are running Docker and joined into a cluster. After that has happened, you continue to run the Docker commands you’re used to, but now they are executed on a cluster by a **swarm manager**. The machines in a swarm can be physical or virtual. After joining a swarm, they are referred to as nodes.

Swarm managers can use several strategies to run containers, such as “emptiest node” – which fills the least utilized machines with containers. Or “global”, which ensures that each machine gets exactly one instance of the specified container. You instruct the swarm manager to use these strategies in the Compose file, just like the one you have already been using.

**Swarm managers are the only machines in a swarm that can execute your commands**, or authorize other machines to join the swarm as workers. **Workers are just there to provide capacity** and do not have the authority to tell any other machine what it can and cannot do.

Docker can be switched into **swarm mode**, and that’s what enables the use of swarms. Enabling swarm mode instantly makes the current machine a swarm manager. From then on, Docker will run the commands you execute on the swarm you’re managing, rather than just on the current machine.

### Set up your swarm

A swarm is made up of multiple nodes, which can be either physical or virtual machines. The basic concept is simple enough: `run docker swarm init` to enable swarm mode and make your current machine a **swarm manager**, then run `docker swarm join` on other machines to have them join the swarm as workers.

### Create a cluster

```
$ docker-machine create --driver virtualbox myvm1
$ docker-machine create --driver virtualbox myvm2
```
**LIST THE VMS AND GET THEIR IP ADDRESSES**

```
$ docker-machine ls

# output
NAME    ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER        ERRORS
myvm1   -        virtualbox   Running   tcp://192.168.99.100:2376           v17.06.2-ce   
myvm2   -        virtualbox   Running   tcp://192.168.99.101:2376           v17.06.2-ce   
```

**INITIALIZE THE SWARM AND ADD NODES**

send commands to your VMs using `docker-machine ssh`. Instruct myvm1 to **become a swarm manager** with `docker swarm init`

```
$ docker-machine ssh myvm1 "docker swarm init --advertise-addr <myvm1 ip>"
Swarm initialized: current node <node ID> is now a manager.

To add a worker to this swarm, run the following command:

  docker swarm join \
  --token <token> \
  <myvm ip>:<port>

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```

**PORTS**

Always run `docker swarm init` and `docker swarm` join with port 2377 (the swarm management port), or no port at all and let it take the default.

The machine IP addresses returned by docker-machine ls include port 2376, which is the Docker daemon port. Do not use this port or you may experience errors.

Join the second VM to the swarm with:

```
$ docker-machine ssh myvm2 "docker swarm join \
--token <token> \
<ip>:2377"

This node joined a swarm as a worker.
```

Run docker node ls on the manager to view the nodes in this swarm:

```
$ docker-machine ssh myvm1 "docker node ls"
ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS
brtu9urxwfd5j0zrmkubhpkbd     myvm2               Ready               Active
rihwohkh3ph38fhillhhb84sk *   myvm1               Ready               Active              Leader
```

## Deploy your app on the swarm cluster

Use the swarm manager to deploy your app by using the same `docker stack deploy` comman, and your **docker-compose.yml**.

you’ll see that the services (and associated containers) have been distributed between both myvm1 and myvm2.

```
$ docker stack ps getstartedlab

ID            NAME                  IMAGE                   NODE   DESIRED STATE
jq2g3qp8nzwx  getstartedlab_web.1   john/get-started:part2  myvm1  Running
88wgshobzoxl  getstartedlab_web.2   john/get-started:part2  myvm2  Running
vbb1qbkb0o2z  getstartedlab_web.3   john/get-started:part2  myvm2  Running
ghii74p9budx  getstartedlab_web.4   john/get-started:part2  myvm1  Running
0prmarhavs87  getstartedlab_web.5   john/get-started:part2  myvm2  Running
```

### Accessing your cluster

You can access your app from the IP address of either `myvm1` or `myvm2`.

The network you created is shared between them and load-balancing. Run `docker-machine ls` to get your VMs’ IP addresses and visit either of them on a browser, hitting refresh (or just curl them).

You’ll see five possible container IDs all cycling by randomly, demonstrating the **load-balancing**.

The reason both IP addresses work is that nodes in a swarm participate in an ingress **routing mesh**. This ensures that a service deployed at a certain port within your swarm always has that port reserved to itself, no matter what node is actually running the container. Here’s a diagram of how a routing mesh for a service called my-web published at port 8080 on a three-node swarm would look:
