+++
title = "Docker Cheat-Sheet"
date = "2024-01-24"
description = ""
+++

## Basics

What is Docker? Docker is a virtualization software that makes developing and deploying applications easier. Docker is written in the GO programming language.

Before docker was invented, every developer had to install and configure all services directly on their operating system on their local machine. However, the installation process is different on different operating systems. Furthermore, many steps are required where something can go wrong.

When using docker, one is working with containers. A container is an isolated environment. E.g. Postgres in a specific version can be packaged with all dependencies and configs inside a container.

the following command fetches a container package from the internet and starts it on your computer.

"docker run postgres"

Advantages:
- command is the same for all operating systems
- command is the same for all services

With Docker, the old problem of _“It only works on my machine”_ is gone.
Each piece of software runs inside its own container, completely separate from your local system. A container will run the same way everywhere — on your laptop, on a server, or in the cloud.
All dependencies (libraries, tools, configurations) are already included inside the container.

This also avoids version conflicts — for example, if two projects need different versions of Python or Node.js, they can each run in their own container without interfering with each other.


## Docker vs. VMs

Do you know virtual machines? I mean the thing where you can run for instance the windows operating system on a MacOS apple device:

{{< figure src="/images/docker/vm.png" caption="With a virtual machine you can run a windows OS on MacOS" width="50%">}}

Docker is something like a VM. Aber mit feinen Unterschieden.

Im folgenden Bild sieht man den Aufbau eines Betriebssystems:

{{< figure src="/images/docker/os_architecture.svg" caption="Architecture of an Operating System" >}}

Layers explained:
- Hardware-Layer (=the actual machine) CPU, Speicher, Festplatten, Netzwerk, Geräte
- OS-Application-Layer (= die Ebene, auf der Programme laufen, die der Benutzer direkt verwendet)
- OS Kernel Layer – Kern des Betriebssystems (Prozessverwaltung, Speicherverwaltung, Treiber, Systemaufrufe). The kernel interacts with hardware and apps (= Middleman)

What is now the difference between a VM and Docker?
A VM virtualizes the whole OS. It has its own application layer and its own Kernel. Docker on the other hand only virtualizes the OS-Application-Layer. That means when comparing docker to a traditional vm:

- Docker is more lightweight and saves disk-space because it only has 1 OS-Layer instead of 2.

- Docker is much faster because 1 layer boots faster than 2 layers.

Another difference: You can run a Linux VM on a windows machine. You cannot do that with docker. Docker is only compatible with Linux distros.

{{< figure src="/images/docker/not_working.svg" caption="Linux based docker images cannot use windows kernel" >}}


The solution: Docker introduced "Docker Desktop" which uses a "Hypervisor Layer" with a lightweight linux distro to provide a Linux-Kernel for MacOS and Windows.

For local development you have to install docker desktop on your mac or windows laptop to run linux based images!


## Docker Architecture

Docker Daemon, Docker Client und Docker Registries erklären.

https://docs.docker.com/get-started/docker-overview/ 


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