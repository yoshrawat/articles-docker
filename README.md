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
