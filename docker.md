# Docker

## Concepts

### Image
An **image** is a lightweight, stand-alone, **executable package** that **includes everything needed to run** a piece of software, including the code, a runtime, libraries, environment variables, and config files.

### Container
A container is a **runtime instance of an image**—what the image becomes in memory when actually executed. It runs completely **isolated** from the host environment by default, only accessing host files and ports if configured to do so.

Containers run apps natively on the host machine’s kernel. They have *better **performance** characteristics than virtual machines* that only get virtual access to host resources through a hypervisor. Containers can get native access, each one running in a discrete process, taking no more memory than any other executable.
## Dockerfile

`Dockerfile` will define what goes on in the environment inside your container. **Access** to resources like **networking interfaces** and disk drives is virtualized inside this environment, which is isolated from the rest of your system, so you have to * **map ports** to the outside world*, and be specific about what files you want to “copy in” to that environment. However, after doing that, you can expect that the build of your app defined in this Dockerfile will behave exactly the same wherever it runs.

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
