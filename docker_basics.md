# Docker — Practical Usage Guide (Linux / Ubuntu)

Single-file reference.
CLI-only.
Copy–paste friendly.
No GUI.
No Docker Desktop.

==================================================
SYSTEM ASSUMPTIONS
==================================================

- OS: Ubuntu 20.04 / 22.04 / 24.04 (x86_64)
- Kernel: ≥ 5.4
- Docker Engine installed
- User added to `docker` group
- Internet access
- Shell: bash / zsh

Verify Docker is usable:

    docker info

If this fails, Docker is not correctly installed or running.

==================================================
DEPENDENCIES (AND WHY THEY MATTER)
==================================================

- Docker Engine (docker): container runtime
- containerd: low-level container manager
- Linux namespaces + cgroups: isolation and limits
- /var/run/docker.sock permissions: CLI access

Check versions:

    docker --version
    docker compose version

==================================================
MENTAL MODEL (MINIMUM REQUIRED)
==================================================

- Image      → Immutable template
- Container  → Running instance of an image
- Volume     → Persistent data storage
- Network    → Container-to-container communication
- Dockerfile → Image build instructions
- Compose    → Multi-container orchestration

Always think in this order:

1. Image
2. Container
3. Data
4. Network

==================================================
CORE WORKFLOW
==================================================

1. Pull or build an image
2. Run a container
3. Inspect logs / exec into container
4. Stop or remove container
5. Persist data using volumes

==================================================
BASIC IMAGE OPERATIONS
==================================================

Pull an image:

    docker pull nginx:latest

List images:

    docker images

Remove an image:

    docker rmi nginx:latest

==================================================
RUNNING CONTAINERS
==================================================

Run a container (detached, port exposed):

    docker run -d -p 8080:80 --name web nginx

Flags explained:

- -d        → run in background
- -p        → host_port:container_port
- --name    → human-readable name

List running containers:

    docker ps

List all containers:

    docker ps -a

==================================================
CONTAINER LIFECYCLE
==================================================

Stop a container:

    docker stop web

Start a container:

    docker start web

Remove a container:

    docker rm web

Force remove (running container):

    docker rm -f web

==================================================
LOGS AND SHELL ACCESS
==================================================

View logs:

    docker logs web

Follow logs:

    docker logs -f web

Open shell inside container:

    docker exec -it web /bin/bash

If bash is missing:

    docker exec -it web /bin/sh

==================================================
VOLUMES (PERSISTENT DATA)
==================================================

Create a named volume:

    docker volume create pgdata

Run PostgreSQL with persistent storage:

    docker run -d \
      --name postgres \
      -e POSTGRES_PASSWORD=pass \
      -v pgdata:/var/lib/postgresql/data \
      postgres:16

List volumes:

    docker volume ls

Remove unused volumes:

    docker volume prune

Why volumes matter:
- Containers are disposable
- Volumes survive restarts and rebuilds
- Databases REQUIRE volumes

==================================================
NETWORKING (CONTAINER COMMUNICATION)
==================================================

Create a user-defined bridge network:

    docker network create app-net

Run containers on the same network:

    docker run -d --name db --network app-net postgres:16
    docker run -d --name api --network app-net my-api-image

Important:
- Containers resolve each other by container name
- localhost does NOT work between containers

==================================================
BUILDING IMAGES (DOCKERFILE)
==================================================

Minimal Dockerfile (Node.js example):

    FROM node:20-alpine
    WORKDIR /app
    COPY package*.json ./
    RUN npm install
    COPY . .
    CMD ["npm","run","start"]

Build image:

    docker build -t my-app:1.0 .

Run image:

    docker run -p 3000:3000 my-app:1.0

==================================================
DOCKER COMPOSE (MULTI-CONTAINER APPS)
==================================================

docker-compose.yml example:

    services:
      db:
        image: postgres:16
        environment:
          POSTGRES_PASSWORD: pass
        volumes:
          - pgdata:/var/lib/postgresql/data

      api:
        build: .
        ports:
          - "3000:3000"
        depends_on:
          - db

    volumes:
      pgdata:

Start stack:

    docker compose up -d

Stop stack:

    docker compose down

Rebuild images:

    docker compose up -d --build

View running services:

    docker compose ps

==================================================
CLEANUP (MANDATORY MAINTENANCE)
==================================================

Remove stopped containers:

    docker container prune

Remove unused images:

    docker image prune

Remove everything unused (DANGEROUS):

    docker system prune -a

==================================================
VERIFICATION CHECKLIST
==================================================

- docker ps shows running containers
- Services accessible via localhost:PORT
- docker logs shows output
- Volumes persist data after restart
- No sudo required for docker commands

==================================================
COMMON FAILURE MODES AND FIXES
==================================================

Permission denied on docker.sock
Cause: user not in docker group
Fix:

    sudo usermod -aG docker $USER
    logout/login

--------------------------------------------------

Cannot connect to Docker daemon
Cause: daemon not running
Fix:

    sudo systemctl start docker
    sudo systemctl status docker

--------------------------------------------------

Port already in use
Cause: host port conflict
Fix:

    -p 8081:80

--------------------------------------------------

Container exits immediately
Cause: CMD crashed
Fix:

    docker logs <container_name>

--------------------------------------------------

Data lost after restart
Cause: no volume attached
Fix:
- Use named volumes (-v)

--------------------------------------------------

Containers cannot talk to each other
Cause: default bridge isolation
Fix:
- Use user-defined network

==================================================
LIMITATIONS
==================================================

- No Kubernetes
- No Docker Desktop
- No rootless Docker
- No production hardening
- No swarm mode

==================================================
RESULT
==================================================

You can now:

- Run databases locally without installing them
- Package applications consistently
- Reproduce environments across machines
- Replace most local dev installs with containers

This is the complete Docker skill set used in real-world development.
