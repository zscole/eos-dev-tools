# EOSIO Development Resources

I had a lot of trouble finding adequate documentation and resources related to EOS development, so I put all of this together for my own use. 

The Dockerfile creates a static binary without any external dependencies. The resulting image should be around 380mb in size and work out of the box as a vanilla EOS build.

You can also pull this image directly from Docker Hub with `docker pull whiteblock/eosio`.

I've included the Makefile for reference. I modified the build script for development and testing purposes and removed any hardcoded requirements for resource allocation.

## Dependencies: 
- Boost
- Cmake
- Clang
- LLVM
- OpenSSL
- secp256k1-zkp


