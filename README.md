# EOSIO Development Resources

I had a lot of trouble finding adequate documentation and resources related to EOS development, so I put all of this together for my own use. The following presents some of the stuff I've figured out that wasn't entirely clear in the [documentation provided by EOS](https://developers.eos.io/).

The Dockerfile creates a static binary without any external dependencies. The resulting image should be around 380mb in size and work out of the box as a vanilla EOS build.

You can also pull this image directly from Docker Hub with `docker pull whiteblock/eosio`.

I've included the `Makefile` and an example `config.init` for reference. I also modified the build script for development and testing purposes and removed any hardcoded requirements for resource allocation.

If you have any issues, please feel free to share them here and if you have any questions, don't hesitate to [contact me on Telegram](https://t.me/zcole). 

## Dependencies

These are provided here for reference and already included in the Docker image, if you're using that. 

- Boost
- Cmake
- Clang
- LLVM
- OpenSSL
- secp256k1-zkp

## Overview

`nodesos` is the primary EOS client which allows users to implement different functionality within their node via the EOS plugin library. Manages account actions and publishing.   

`cleos` is the command line interface which acts as a bridge between the blockchain network and the user's local environment via REST API. Accesses wallets which store public-private key pairs and interacts with `nodeos` and `keosd`. Launches `keosd` when you run it with a subcommand. If `keosd` is still running from a previous instance, it'll give you problems, which is annoying and weird, so run `pkill keosd` to kill all instances or `ps aux | grep keosd` and then kill the process ID. `cleos` is also responsible for creating accounts, which is done by sending a request to `nodeos`. 
 
`keosd` is a wallet management utility. When creating a new wallet, the master password is output to the command line in hexadecimal format beginning with a PW prefix, like so: `PW5JQwauDUuAWFyCYdMCuqGRc28oN3PcPLivfWvBxkhjYZ7Gw53a8`. Be sure to save this because it won't be stored anywhere else.     

### Plugins

These plugins can either be specified for automatic implementation within the `config.ini` file or manually specified as a user flag when launching `nodeos`. 

- `bnet_plugin` is used to ensure synchronization between multiple nodes. [Details](https://developers.eos.io/eosio-nodeos/docs/bnet_plugin)  
`chain_plugin` manages the collection and processing of blockchain related data. [Details](https://developers.eos.io/eosio-nodeos/docs/chain_plugin)  
- `http_plugin` manages the RPC API. [Details](https://developers.eos.io/eosio-nodeos/docs/http_plugin)  
`chain_api_plugin` acts as a bridge between the `chain_plugin` and `http_plugin` to allow RPC API interaction. [Details](https://developers.eos.io/eosio-nodeos/docs/chain_api_plugin)  
- `net_plugin` manages a node's ability to connect to the outside world, interact within the network, and peer with other nodes. [Details](https://developers.eos.io/eosio-nodeos/docs/net_plugin)  
- `producer_plugin` allows a node to act as block producer. [Details](https://developers.eos.io/eosio-nodeos/docs/producer_plugin)
- `wallet_plugin` allows a node to interact with a wallet.  

### Commands 

- `cleos create key -f $FILENAME` creates a new key and outputs it to the specified file. If you want to output your key to the command line, replace the `-f $FILENAME` flag with `--to-console`.  

- `cleos wallet create -n <name> --to-console` creates a new wallet and outputs to command line. 

## Starting A New Blockchain

Starting a new blockchain for testing/development purposes is a great way to learn about how a system works in a safe and controlled environment.  
The following instructions will help you get started and set up your own private environment. These instructions assume you're using the Dockerfile included in this repo and deploying your new blockchain network locally. 

### Create A New Key

To get started, you first need to create a new key via `cleos`, so run the following command:

`cleos create key --to-console`

The output will be a new public/private key pair. Here is an example of the expected output:

`Private key: 5JW8h6Y69SgqpxiJAZdvr2QDtxs1VmHtSTtrZX5qwFPkgcb8pgL`  
`Public key: EOS5sqheHF2NnJF1s4ZbfC4rsqWF7vVrqwTenbtHfY5mRgcJ9Xird`

 The `--to-console` flag displays the output on the console, however, if you wish to output these values to a particular file, replace `--to-console` with `-f $filename` and change $filename to whatever file you'd like to save it as. 

### Configure The Genesis File

Every node in your network is going to reference a genesis node, which is the node that created the first block in the blockchain. Check out the genesis.json file in this repo and take note of two values - `initial_chain_id` and `initial_key`. 

Take the public key you generated in the previous step and insert it as a string in the `initial_key` field. The `initial_chain_id` field seems to be arbitrary, but if you're planning on deploying multiple blockchains, it's probably best practice to assign a unique value to each build for the sake of sanity. The expected input is a hex string. 

Save your `genesis.json` file wherever you want. If you're running EOS in a docker container, `cd` into the directory where you saved it and run the following command to copy it over to the docker container:

`docker cp genesis.json <$CONTAINER>:/genesis.json`

Be sure to replace $CONTAINER with whatever your container name is. If you're not sure what the name of your container is, run `docker container ls` to check out a list of running containers. 

### Config.ini

In your `config.ini` file, along with whatever other values you wish to specify the path to this file as the value for `genesis-json = /$PATH`. Be sure to replace $PATH with the appropriate path. Like you did with the genesis.json file, `cp` it into your Dockerfile. If you're going to use the same values in each build, it might be best to change the Dockerfile to copy your custom `config.ini` file from your host machine to container during the build process. 

THIS DOCUMENT IS STILL BEING EDITED. 









