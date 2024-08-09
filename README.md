Apache SkyWalking
==========

<img src="http://skywalking.apache.org/assets/logo.svg" alt="Sky Walking logo" height="90px" align="right" />

**SkyWalking**: an APM (Application Performance Monitoring) system, especially designed for
microservices, cloud native and container-based architectures.

[![GitHub stars](https://img.shields.io/github/stars/apache/skywalking.svg?style=for-the-badge&label=Stars&logo=github)](https://github.com/apache/skywalking)
[![Twitter Follow](https://img.shields.io/twitter/follow/asfskywalking.svg?style=for-the-badge&label=Follow&logo=twitter)](https://twitter.com/AsfSkyWalking)
[![Maven Central](https://img.shields.io/maven-central/v/org.apache.skywalking/apache-skywalking-apm.svg)](http://skywalking.apache.org/downloads/)

<img src="https://skywalking.apache.org/images/home/architecture.svg?t=20220516"/>

# SkyWalking Azure Deployment Guide

This will help you setting up the SkyWalking project on an Azure VM using Docker for installation.

## Prerequisites

- **Java**: Java Development Kit (JDK) 11 or 20
- **Maven**: Install Maven from Maven's official site
- **Git**: Make sure Git is installed on your local machine.
- **Docker**: Ensure Docker and Docker Compose are installed.
- **Azure CLI**: Install the Azure CLI to manage Azure resources.

## 1. Clone the Repository

Clone the SkyWalking repository to your local machine:
```sh
git clone https://github.com/your-repo/skywalking.git
cd skywalking
```

## Checkout the Specific Branch
```sh
git checkout skywalking-azure
```

## Initialize Submodules
```sh
git submodule update --init --recursive
```

## Prepare Docker Environment
```sh
make docker
```

## Build and Run the Containers
```sh
docker-compose --profile elasticsearch up -d
```

## Log in to Azure
```sh
az login
```

## Create a Resource Group
```sh
az group create --name skywalking-rg --location centralindia
```

## Create an Azure VM
```sh
az vm create --resource-group skywalking-rg --name skywalking-vm --image UbuntuLTS --admin-username azureuser --generate-ssh-keys
```

## Open Port 8080 on the Azure VM
```sh
az vm open-port --resource-group skywalking-rg --name skywalking-vm --port 8080
```

## SSH into the Azure VM
```sh
ssh azureuser@your-vm-ip
```

## Pull the Docker Image inside the VM
```sh
docker pull your-docker-image
```

## Run the Docker Container
```sh
docker run -d -p 8080:8080 your-docker-image
```
## Configure GitHub Secrets

Add the secrets
```sh
SSH_PRIVATE_KEY : key used for accessing the VM
VM_IP: The public IP address of your VM.
VM_USER: The SSH username for the VM.
```
## Create the GitHub Container
Generate a github Personal access token :
store its value in PAT
```sh
echo $PAT | docker login ghcr.io -u your-username --password-stdin
```
