# Docker Custom Networking & Bind Mounts

## What I Built
Created a custom Docker network to enable container-to-
container communication by name, and demonstrated a bind 
mount for live file editing between my laptop and a 
running container.

## Tools Used
- Docker Desktop
- Git Bash (Windows)
- Nginx

## Part 1: Custom Docker Networking

### What is a Custom Network?
By default, Docker containers are isolated from each 
other. A custom network allows containers to discover 
and communicate with each other using their container 
NAME as a hostname — no IP addresses needed.

### Commands Used
docker network create devops-network
docker run -d --name backend-container --network devops-network nginx
docker run -d --name frontend-container --network devops-network nginx
docker exec frontend-container curl -s http://backend-container

### Key Result
frontend-container successfully reached backend-container 
using its NAME — confirming Docker's built-in DNS 
resolution works between containers on the same network.

## Part 2: Bind Mounts

### What is a Bind Mount?
Unlike a Docker-managed volume, a bind mount points 
directly to a specific folder on the host machine (my 
laptop). Any changes made to files on the laptop are 
immediately reflected inside the running container — 
no rebuild required.

### Commands Used
mkdir -p ~/bind-mount-demo
docker run -d --name bind-demo -p 8090:80 \
  -v /c/Users/user/bind-mount-demo:/usr/share/nginx/html nginx

### Real Issue Solved
Git Bash on Windows automatically converts Unix-style 
paths, which broke the bind mount initially. Fixed by 
prefixing commands with MSYS_NO_PATHCONV=1 to prevent 
Git Bash from mangling the container path.

MSYS_NO_PATHCONV=1 docker run -d --name bind-demo -p 8090:80 \
  -v /c/Users/user/bind-mount-demo:/usr/share/nginx/html nginx

### Key Result
Confirmed my custom HTML file was served directly from 
my laptop folder, live, inside the container.

## Key Lessons
- Custom networks enable container-to-container 
  communication by name — exactly how Docker Compose 
  services talk to each other internally
- Bind mounts are ideal for local development — edit 
  code on your machine, see changes instantly in the 
  container
- Volumes vs Bind Mounts: volumes are Docker-managed 
  and portable; bind mounts give direct control over 
  an exact host folder
- Windows/Git Bash can mangle Unix-style paths — 
  MSYS_NO_PATHCONV=1 resolves this

## DevOps Connection
Custom networking is the exact mechanism behind Docker 
Compose multi-container apps. Bind mounts are used daily 
by developers for live-reload development environments 
before code is containerized for production.
