+++
title = "Docker Cheat-Sheet"
date = "2024-01-24"
description = ""
+++

## Basics

What is Docker? 

Docker is a virtualization tool that makes developing and deploying applications much easier.

Before Docker existed, developers had to install and configure all required services directly on their local machines. This process could be tricky because:

- Installation steps differ between operating systems.
- Many steps are required, and errors can easily occur.

With Docker, you work with containers. A container is an isolated environment that includes everything a service needs to run.

For example, you can package Postgres in a container with a specific version, along with all its dependencies and configuration. You can then start it on your computer with a single command:

```bash
docker run postgres
```

Advantage of Docker:

- The same commands work across all operating systems.
- The same commands work for all services.

With Docker, the old problem of _“It only works on my machine”_ is gone.
Each piece of software runs inside its own container, completely separate from your local system. A container will run the same way everywhere — on your laptop, on a server, or in the cloud.
All dependencies (libraries, tools, configurations) are already included inside the container.

This also avoids version conflicts — for example, if two projects need different versions of Python or Node.js, they can each run in their own container without interfering with each other.


## Docker vs. VMs


Do you know virtual machines?

A virtual machine (VM) allows you to run an operating system, like Windows, on a different host system, like MacOS:

{{< figure src="/images/docker/vm.png" caption="With a virtual machine you can run a Windows OS on MacOS" width="50%">}}

Docker is similar to a VM — but with some important differences.

Here’s a look at the structure of an operating system:

{{< figure src="/images/docker/os_architecture.svg" caption="Architecture of an Operating System" >}}


- Hardware Layer: The actual machine — CPU, memory, storage, network, devices

- OS Application Layer: Where user programs run

- OS Kernel Layer: Core of the OS — manages processes, memory, drivers, and system calls. The kernel acts as a middleman between hardware and applications.


A VM virtualizes the entire OS. It has its own kernel and application layer.

However Docker only virtualizes the OS Application Layer.


Implications:

- Docker is lighter and uses less disk space because it shares the host OS kernel (it only has one OS-Layer instead of two).

- Docker starts faster because it boots only one layer instead of two.

Another difference: You can run a Linux VM on a windows machine. You cannot do that with docker. Docker is only compatible with Linux distros.

{{< figure src="/images/docker/not_working.svg" caption="Linux based docker images cannot use windows kernel" >}}

The solution: Docker introduced "Docker Desktop" which uses a "Hypervisor Layer" with a lightweight linux distro to provide a Linux-Kernel for MacOS and Windows.

For local development you have to install docker desktop on your mac or windows laptop to run linux based images!


## Docker Architecture

Docker Daemon, Docker Client und Docker Registries erklären.

https://docs.docker.com/get-started/docker-overview/ 


## Docker Images

A Docker image is an executable artifact. It is a ready-to-run package that contains everything an application needs to work.

It includes:

- Application source code (e.g., a JavaScript app)
- Dependencies and services (e.g., Node.js, npm)
- OS layer (usually Linux)
- Configuration such as environment variables, directories, and files

Docker images are usually downloaded from DockerHub, making it easy to run applications without manually setting up the environment.

## Docker Containers

We need to start the image somewhere --> in a container!

A container is where a Docker image actually runs.
- It starts and executes the application from the image.
- A container is essentially a running instance of an image.
- You can run multiple containers from the same image at the same time.

## CD 💿 and CD-Player 📻

You can think of a Docker image like a CD:

The CD contains all the content you need — the music, album art, and instructions for playing.

Similarly, a Docker image contains everything an application needs — code, dependencies, configuration, and the OS layer.

A Docker container is like a CD playing in a CD player:

The CD itself doesn’t do anything until it’s in the player.

Once the CD is in the player, the music starts playing — just like a container runs the application from the image.

You can put the same CD in multiple CD players and play it simultaneously, just like you can run multiple containers from the same image.

This analogy makes it easy to see the difference: image = static package, container = running instance.

## Container-Basics

Container starten, stoppen, löschen

Arbeiten mit Images (docker pull, docker build, docker rmi)

DockerHub und Private Registries (nur Überblick)

Interaktive Container (Shell starten mit docker exec)

Persistenz mit Volumes und Bind Mounts (Grundprinzip verstehen)

## Docker Images

Aufbau eines Images (Union File System, Layers – nur grob verstehen)

Dockerfile verstehen und schreiben (Grundlagen: FROM, RUN, COPY, CMD, EXPOSE)

Basis-Images vs. Scratch-Image (nur Konzept)

Image Tagging und Versionierung (:latest, :1.0)

(Best Practices & Multi-Stage-Builds kannst du überspringen, das kommt später bei Bedarf.)

## Netzwerke

Container-Netzwerke (bridge)

Ports exposen und weiterleiten (-p 8080:80)

Container miteinander verbinden (z. B. App + DB im selben Network)

DNS-Auflösung innerhalb von Docker-Netzwerken (nur Überblick)

(Custom Networks erstellen = nice-to-have, aber kein Muss für Kubernetes-Basis.)

## Storage & Volumes

Unterschied Volumes vs. Bind Mounts

Named Volumes (nur Konzept)

Persistenz-Prinzip verstehen (damit du in Kubernetes später PersistentVolumes verstehst)

## Docker Compose (optional, nur Überblick)

Einführung in docker-compose.yml

Mehrere Container orchestrieren (z. B. App + DB)

Umgebungsvariablen und .env-Dateien

👉 Compose brauchst du nicht in der Tiefe, aber ein kurzer Überblick hilft dir, den Schritt zu Kubernetes zu verstehen.