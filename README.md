# articles-docker

## Step 1 - Running A Container
	docker run -d redis:latest

 	By default, Docker will run a command in the foreground. To run in the background, the option -d needs to be specified.
    	docker run -d redis.
## Step 2 - Finding Running Containers
The launched container is running in the background, the docker ps command lists all running containers, the image used to start the container and uptime.
This command also displays the friendly name and ID that can be used to find out information about individual containers.
The command 

		docker inspect <friendly-name|container-id> 
		provides more details about a running container, such as IP address.
		
The command 
	docker logs <friendly-name|container-id> will display messages the container has written to standard error or standard out. 

## Step 3 - Accessing Redis
Jane is happy that Redis is running, but is surprised that she cannot access it. The reason is that each container is sandboxed. If a service needs to be accessible by a process not running in a container, then the port needs to be exposed via the Host.

Once exposed, it is possible to access the process as if it were running on the host OS itself.

Jane knows that by default, Redis runs on port 6379. She has learned that by default other applications and library expect a Redis instance to be listening on the port.

Task
After reading the documentation, Jane discovers that ports are bound when containers are started using -p <host-port>:<container-port> option. Jane also discovers that it's useful to define a name when starting the container, this means she doesn't have to use Bash piping or keep looking up the name when trying to access the logs.

Jane finds the best way to solve her problem of running Redis in the background, with a name of redisHostPort on port 6379 is using the following command 
	
		docker run -d --name redisHostPort -p 6379:6379 redis:latest

Protip
By default, the port on the host is mapped to 0.0.0.0, which means all IP addresses. You can specify a particular IP address when you define the port mapping, for example, -p 127.0.0.1:6379:6379


## Step 4 - Accessing Redis
The problem with running processes on a fixed port is that you can only run one instance. Jane would prefer to run multiple Redis instances and configure the application depending on which port Redis is running on.

Task
After experimenting, Jane discovers that just using the option -p 6379 enables her to expose Redis but on a randomly available port. She decides to test her theory using  

	docker run -d --name redisDynamic -p 6379 redis:latest

While this works, she now doesn't know which port has been assigned. Thankfully, this is discovered via docker port redisDynamic 6379

Jane also finds that listing the containers displays the port mapping information, docker ps


## Step 5 - Persisting Data
After working with containers for a few days, Jane realises that the data stored keeps being removed when she deletes and re-creates a container. Jane needs the data to be persisted and reused when she recreates a container.

Containers are designed to be stateless. Binding directories (also known as volumes) is done using the option -v <host-dir>:<container-dir>. When a directory is mounted, the files which exist in that directory on the host can be accessed by the container and any data changed/written to the directory inside the container will be stored on the host. This allows you to upgrade or change containers without losing your data.

Task
Using the Docker Hub documentation for Redis, Jane has investigated that the official Redis image stores logs and data into a /data directory.

Any data which needs to be saved on the Docker Host, and not inside containers, should be stored in
		
		/opt/docker/data/redis.

The complete command to solve the task is 

	docker run -d --name redisMapped -v /opt/docker/data/redis:/data redis

Protip
Docker allows you to use $PWD as a placeholder for the current directory.

## Step 6 - Running A Container In The Foreground
Jane has been working with Redis as a background process. Jane wonders how containers work with foreground processes, such as ps or bash.

Previously, Jane used the -d to execute the container in a detached, background, state. Without specifying this, the container would run in the foreground. If Jane wanted to interact with the container (for example, to access a bash shell) she could include the options -it.

As well as defining whether the container runs in the background or foreground, certain images allow you to override the command used to launch the image. Being able to replace the default command makes it possible to have a single image that can be re-purposed in multiple ways. For example, the Ubuntu image can either run OS commands or run an interactive bash prompt using /bin/bash

Example
The command 

	docker run ubuntu ps

launches an Ubuntu container and executes the command ps to view all the processes running in a container.

Using 
	
	docker run -it ubuntu bash 

allows Jane to get access to a bash shell inside of a container.
