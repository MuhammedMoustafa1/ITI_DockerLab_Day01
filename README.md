# ITI - Docker Lab 🐋

## Task 1: Working with Docker Hello-world Image
### Objective
Learn how to run a container using the hello-world image and manage containers and images.

### Steps
#### 1. Run a Container with hello-world Image
```bash
docker run hello-world
```
#### 2. Check Container Status and Explain
```bash
docker ps -a
docker ps only did not show hello-world image
```
#### 3. Start the Stopped Container
```bash
docker start ebdf48827469 

```
#### 4. Remove the Container
```bash
docker rm ebdf48827469 
```
#### 5. Remove the Image
```bash
docker rmi hello-world
```
---

## Task 2: Running Container with Ubuntu Image
### Objective
Run an Ubuntu container in interactive mode, create a file inside it, and manage containers.

### Steps
#### 1. Run Ubuntu Container in Interactive Mode
```bash
docker run -it ubuntu

```
#### 2. Create a File inside the Container
```bash
touch hello-docker

```
#### 3. Stop and Remove the Container
```bash
exit
docker stop f58aec1b182b
docker rm f58aec1b182b
```
#### 4. Check File Status
```bash
docker ps -a
File is removed
```
#### 5. What happened to hello-docker file?
```bash
docker ps -a
file is deleted with container
```
#### 6. Remove All Stopped Containers
```bash
docker rm f58aec1b182b
```
#### 7. Bonus: Remove All Containers in One Command
```bash
```

---
## Task 3: Creating a Custom Nginx Docker Image
### Objective
Create a custom Docker image using Nginx and a local HTML file.

### Steps
#### 1. Create a Local HTML File
```bash
vi index.html
```
#### 2. Write Dockerfile and Copy the HTML file to the Docker Image
```bash
vi Dockerfile
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/index.html
```
#### 3. Run Container with New Image
```bash
docker build -t nginx-mohamed .
```

#### 4. Test the Container, open your browser and navigate to http://localhost:8088 to check if everything is okay
```bash
http://localhost:8088/ my name appeared in this url
```

# ITI-Docker-Lab 2 🐋

## Task 1: Run a container using nginx image, and mount a directory from your host into the Docker container. 
### example: /home/samy/nginx:/home/nginx (bind mount)

#### 1-create a directory and file iti.txt in it 
```bash
mkdir nginx_bindMount
cd nginx_bindMount/
touch iti.txt
```
#### 2- Run container using nginx image
```bash
docker run -d --name nginx_bindMount -v /root/nginx_bindMount:/usr/share/nginx/html nginx-talaat
```

#### 3-check if the file is mounted on the machine
```bash
docker exec -it nginx_bindMount bash
cd /usr/share/nginx/html/
ls
we find iti.txt present indicating that it is mounted into the container
```

---

## Task 2
#### 1-Create 2 docker network (net-1 & net-2)
```bash
docker network create net1
docker network create net2
```
#### 2-Run 2 new containers using nginx:alpine image, and attach the net-1 to them.
```bash
docker run -d --name nginx-1 nginx:alpine
docker run -d --name nginx-2 nginx:alpine
docker network connect net-1 nginx-1
docker network connect net-1 nginx-2
```

#### 3-Run another 1 new containers using nginx:alpine image, and attach the net-2 to them.
```bash
docker run -d --name nginx-3 nginx:alpine
docker network connect net-2 nginx-3
```

#### 4-Inspect the 3 containers to know their IPs and write them aside
```bash
docker inspect -f '{{.NetworkSettings.Networks.net1.IPAddress}}' nginx-1
172.18.0.2
docker inspect -f '{{.NetworkSettings.Networks.net1.IPAddress}}' nginx-2
172.18.0.3
docker inspect -f '{{.NetworkSettings.Networks.net2.IPAddress}}' nginx-3
172.19.0.2
```

#### 5-Enter a container in the net-1 network and try to ping a container in the net-2 network (What do you notice?)
```bash
docker exec -t nginx-1 bash
ping 172.19.0.02
the ping fails because the two containers are in different networks.
```

#### 6-Enter a container in the net-1 network and try to ping the other container in the same network (What do you notice?)
```bash
docker exec -t nginx-1 bash
ping 172.18.0.03
the ping is successful because the two containers are in the same networks.
```

---

## Task 3 Explain the difference between Docker volumes and Bind Mount.Explain the difference between Docker volumes and Bind Mount.

### Docker volumes
#### 1-It is stored within a directory on the Docker host.
#### 2-Managed by Docker and are isolated from the core
#### 3-functionality of the host machine.
#### 4-Can be mounted into multiple containers simultaneously.
#### 5-Deleting a container does not delete the volume


### Bind Mount
#### 1- Bind mounts have limited functionality compared to volumes.
#### 2-When you use a bind mount, a file or directory on the host machine is mounted into a container.
#### 3-The file or directory is referenced by its full path on the host machine.
#### 4-It is created on demand if it does not yet exist.
#### 5-You can’t use Docker CLI commands to directly manage bind mounts.
































