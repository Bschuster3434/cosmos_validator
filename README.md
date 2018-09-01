# cosmos_validator
A repository for the data required for setting up a cosmos repository

# Second Attempt at Install

## Documentation
https://gist.github.com/zramsay/35e158dab11be5380c03b3489ef0e43e
GCC
https://gist.github.com/application2000/73fd6f4bf1be6600a2cf9f56315a2d91
Cosmos Node Setup
https://cosmos.network/docs/getting-started/full-node.html#setting-up-a-new-node
Validator Setup
https://cosmos.network/docs/validators/validator-setup.html#running-a-validator-node

## Steps
-Created a Digital Ocean Droplet and created Ubuntu 16.04.5 LTS Instance
-Ran Install script from Documenation (with edits for adding gcc)

#!/usr/bin/env bash

#XXX: this script is intended to be run from a fresh Digital Ocean droplet

sudo apt-get update -y
sudo apt-get upgrade -y
sudo apt-get install -y make
sudo apt-get install build-essential software-properties-common -y
sudo add-apt-repository ppa:ubuntu-toolchain-r/test -y
sudo apt-get install gcc-snapshot -y

#get and unpack golang
curl -O https://storage.googleapis.com/golang/go1.10.linux-amd64.tar.gz
tar -xvf go1.10.linux-amd64.tar.gz

#move binary and add to path
mv go /usr/local
echo "export PATH=\$PATH:/usr/local/go/bin" >> ~/.profile

#create the goApps directory, set GOPATH, and put it on PATH
mkdir goApps
echo "export GOPATH=/root/goApps" >> ~/.profile
echo "export PATH=\$PATH:\$GOPATH/bin" >> ~/.profile

source ~/.profile

#get the code and move into repo
REPO=github.com/cosmos/cosmos-sdk
go get $REPO
cd $GOPATH/src/$REPO

#Install GCC

#build
git checkout master
make get_tools
make get_vendor_deps
make install

#Create the Node
gaiad init --name <your_custom_name>
mkdir -p $HOME/.gaiad/config
curl https://raw.githubusercontent.com/cosmos/testnets/master/latest/genesis.json > $HOME/.gaiad/config/genesis.json

#Add seeds
nano $HOME/.gaiad/config/config.toml
seeds = "718145d422a823fd2a4e1e36e91b92bb0c4ddf8e@gaia-testnet.coinculture.net:26656,5922bf29b48a18c2300b85cc53f424fce23927ab@67.207.73.206:26656,7c8b8fd03577cd4817f5be1f03d506f879df98d8@gaia-7000-seed1.interblock.io:26656,a28737ff02391a6e00a1d3b79befd57e68e8264c@gaia-7000-seed2.interblock.io:26656,987ffd26640cd03d08ed7e53b24dfaa7956e612d@gaia-7000-seed3.interblock.io:26656"


# First Attempt at Install: Failed

## Documentation
- Created a Vagrant box with the profile:
https://app.vagrantup.com/ubuntu/boxes/trusty64
- Once in the box, guide for building the cosmos validator:
https://cosmos.network/docs/getting-started/installation.html
- Guide for installing GO:
https://golang.org/doc/install
https://medium.com/@patdhlk/how-to-install-go-1-9-1-on-ubuntu-16-04-ee64c073cd79
- Guide to install GIT
https://www.liquidweb.com/kb/install-git-ubuntu-16-04-lts/

## Steps
- Follow the instructions for setting up the Vagrant box and ssh into it.
- Prep the Environment
-- su (password: vagrant)
-- apt-get update
-- apt-get -y upgrade
-- apt-get install -y make
-- apt-get install -y git-core
- Install Go golang
-- curl -O https://storage.googleapis.com/golang/go1.9.1.linux-amd64.tar.gz
-- tar -xvf go1.9.1.linux-amd64.tar.gz
-- mv go /usr/local
- Set the environment variables for GO
-- nano ~/.profile
-- Add the line "export PATH=$PATH:/usr/local/go/bin"
-- Save and close the profile
-- source ~/.profile
-- mkdir -p $HOME/go/bin
-- export GOPATH=$HOME/go
-- export GOBIN=$GOPATH/bin
-- export PATH=$PATH:$GOBIN
- Install Cosmos SDK
-- mkdir -p $GOPATH/src/github.com/cosmos
-- cd $GOPATH/src/github.com/cosmos
-- git clone https://github.com/cosmos/cosmos-sdk
-- cd cosmos-sdk && git checkout master
-- make get_tools
-- make get_dev_tools
-- make get_vendor_deps
-- make install

## Error
go install -tags "netgo ledger" -ldflags "-X github.com/cosmos/cosmos-sdk/version.GitCommit=416181b" ./cmd/gaia/cmd/gaiad
go build github.com/cosmos/cosmos-sdk/cmd/gaia/cmd/gaiad: /usr/local/go/pkg/tool/linux_amd64/link: signal: killed
make: *** [install] Error 1
