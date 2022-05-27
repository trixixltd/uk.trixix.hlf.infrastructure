# uk.trixix.hlf.infrastructure
Contains artefacts for setting up Hyperledger Fabric network

The purpose of this repository is to build a Hyperledger Fabric (HLF) network based on HLF version 2.4.
The objecive is to create three environments
1. Development
2. Test
3. Pre-Production

The network components will be hosted using docker

# Development Environment
This network will consist of one RAFT orderer node from one orderer organisation, two peer nodes from two peer organisations (each contributing one peer orderer node).
[image]

Before getting started with HLF the following are the required
## Setup
Ubuntu 20.04 (1 CPU, 2GB RAM, 50GB SSD)

### Preperation
```
# Update the OS: 
sudo apt update
sudo apt upgrade

# Install some useful helperss: 
sudo apt install tree
sudo apt install jq
sudo apt install gcc
sudo apt install make

# Set correct timezone
timedatectl set-timezone Europe/London

# Install the latest version of cURL, if not already installed
sudo apt-get install curl
```

### Install Docker
The following steps are required to install docker on Ubuntu. Reference: https://docs.docker.com/engine/install/ubuntu/

```
# set up the repository
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

# add Docker’s official GPG key
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# set up the stable repository
echo \
"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# install docker engine
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin

# check the docker version
docker --version
```

### Install Docker Compose
Reference https://docs.docker.com/compose/install/

```
# Install the latest version
sudo apt-get update
sudo apt-get install docker-compose-plugin

# Verify that Docker Compose is installed correctly by checking the version
docker compose version

# Make sure the Docker daemon is running.
sudo systemctl start docker

# [Optionally] If you want Docker daemon to start when the system starts, use the followings:
sudo systemctl enable docker
sudo usermode -a 0G docker <userUserName>
```

### Install Go Language
Install the letest version of Go. This is required because will be writing chaincodes and client applications in Go.
```
# Remove any previous Go installation by deleting the /usr/local/go folder
rm -rf /usr/local/go

# Download GO and extract it into /usr/local, creating a fresh Go tree in /usr/local/go
wget -c https://go.dev/dl/go1.18.2.linux-amd64.tar.gz -O - | tar -xz -C /usr/local

# add the go binary to the path
export PATH=$PATH:/usr/local/go/bin:/root/fabric/fabric-samples/bin
echo 'export PATH="$PATH:/usr/local/go/bin:/root/fabric/fabric-samples/bin"' >> $HOME/.profile

# set the GOPATH env var to the base fabric workspace folder
export GOPATH=$HOME/workspace/chaincode/go
echo 'export GOPATH="$HOME/workspace/chaincode/go"' >> $HOME/.profile

```

### Install NodeJS and npm
This is required because will be writing chaincodes and client applications in NodeJS.
```
sudo apt install nodejs npm

```

### Install Hyperledger Binaries, Docker Images and Samples
```
mkdir -p /opt/hyperledger/fabric
cd /opt/hyperledger/fabric

# Download Fabric version 2.4.3 and CA version 1.5.2
curl -sSL https://bit.ly/2ysbOFE | bash -s -- 2.4.3 1.5.2

# For the latest production ready release, omit all version identifiers
# curl -sSL https://bit.ly/2ysbOFE | bash -s

# check downloaded images
docker images

# check the bin cmd
peer version
```

# Test Environment
This network will consist of one RAFT orderer node from one orderer organisation, and two peer nodes from two peer organisations (each contributing one peer orderer node), initially. Additional orderers and peers will be added as part of the testing. 
[image]

# Pre-Production Environment
This network will consist of three RAFT orderer nodes and three peer nodes from three different organisations.
[image]

