# EOSIO Development Resources

I had a lot of trouble finding adequate documentation and resources related to EOS development, so I put all of this together for my own use. 

The Dockerfile creates a static binary without any external dependencies. The resulting image should be around 380mb in size and work out of the box as a vanilla EOS build.

You can also pull this image directly from Docker Hub with `docker pull whiteblock/eosio`.

I've included the `Makefile` and an example `config.init` for reference. I also modified the build script for development and testing purposes and removed any hardcoded requirements for resource allocation.

## Dependencies: 
- Boost
- Cmake
- Clang
- LLVM
- OpenSSL
- secp256k1-zkp

## Overview

`nodesos` is the primary EOS client which allows users to implement different functionality within their node via the EOS plugin library.   
`cleos` is the command line interface which acts as a bridge between the blockchain network and the user's local environment.  
`keosd` is a wallet management utility.    

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

- `./cleos create key -f $FILENAME` creates a new key and outputs it to the specified file. If you want to output your key to the command line, replace the `-f $FILENAME` flag with `--to-console`.  






