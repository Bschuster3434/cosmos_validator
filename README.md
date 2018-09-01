# cosmos_validator
A repository for the data required for setting up a cosmos repository


##First Attempt at Install: Failed
#Documentation
- Created a Vagrant box with the profile:
https://app.vagrantup.com/ubuntu/boxes/trusty64
- Once in the box, guide for building the cosmos validator:
https://cosmos.network/docs/getting-started/installation.html
- Guide for installing GO:
https://golang.org/doc/install
https://medium.com/@patdhlk/how-to-install-go-1-9-1-on-ubuntu-16-04-ee64c073cd79
- Guide to install GIT
https://www.liquidweb.com/kb/install-git-ubuntu-16-04-lts/

#Steps
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

go install -tags "netgo ledger" -ldflags "-X github.com/cosmos/cosmos-sdk/version.GitCommit=416181b" ./cmd/gaia/cmd/gaiad
go build github.com/cosmos/cosmos-sdk/cmd/gaia/cmd/gaiad: /usr/local/go/pkg/tool/linux_amd64/link: signal: killed
make: *** [install] Error 1
