Bitcoind for Docker
===================

[![Docker Stars](https://img.shields.io/docker/stars/kylemanna/bitcoind.svg)](https://hub.docker.com/r/ayratbadykov/bitcoind/)
[![Docker Pulls](https://img.shields.io/docker/pulls/kylemanna/bitcoind.svg)](https://hub.docker.com/r/ayratbadykov/bitcoind/)

This is a fork of `kylemanna/docker-bitcoind`. The only difference is you can configure your bicoin node with env variables by setting `CONFIG_FROM_ENV = true`. For example, in `docker-compose` file:

```
  name:
    image: ayratbadykov/bitcoind:latest
    environment:
      RPCPASSWORD: bitcoinrpc
      RPCUSER: bitcoinrpc
      DISABLEWALLET: 0
      CONFIG_FROM_ENV: "true"
      TESTNET: 0
      REGTEST: 1
      SERVER: 1
      RPCBIND: ":18443"
      RPCALLOWIP: "0.0.0.0/0"
    ports:
      - 18443:18443/tcp
      - 18444:18444/tcp
    expose:
      - "18444"
```


Requirements
------------

* Physical machine, cloud instance, or VPS that supports Docker (i.e. [Vultr](http://bit.ly/1HngXg0), [Digital Ocean](http://bit.ly/18AykdD), KVM or XEN based VMs) running Ubuntu 14.04 or later (*not OpenVZ containers!*)
* At least 100 GB to store the block chain files (and always growing!)
* At least 1 GB RAM + 2 GB swap file

Recommended and tested on unadvertised (only shown within control panel) [Vultr SATA Storage 1024 MB RAM/250 GB disk instance @ $10/mo](http://bit.ly/vultrbitcoind).  Vultr also *accepts Bitcoin payments*!


Really Fast Quick Start
-----------------------

One liner for Ubuntu 14.04 LTS machines with JSON-RPC enabled on localhost and adds upstart init script:

    curl https://raw.githubusercontent.com/kylemanna/docker-bitcoind/master/bootstrap-host.sh | sh -s trusty


Quick Start
-----------

1. Create a `bitcoind-data` volume to persist the bitcoind blockchain data, should exit immediately.  The `bitcoind-data` container will store the blockchain when the node container is recreated (software upgrade, reboot, etc):

        docker volume create --name=bitcoind-data
        docker run -v bitcoind-data:/bitcoin/.bitcoin --name=bitcoind-node -d \
            -p 8333:8333 \
            -p 127.0.0.1:8332:8332 \
            kylemanna/bitcoind

2. Verify that the container is running and bitcoind node is downloading the blockchain

        $ docker ps
        CONTAINER ID        IMAGE                         COMMAND             CREATED             STATUS              PORTS                                              NAMES
        d0e1076b2dca        kylemanna/bitcoind:latest     "btc_oneshot"       2 seconds ago       Up 1 seconds        127.0.0.1:8332->8332/tcp, 0.0.0.0:8333->8333/tcp   bitcoind-node

3. You can then access the daemon's output thanks to the [docker logs command]( https://docs.docker.com/reference/commandline/cli/#logs)

        docker logs -f bitcoind-node

4. Install optional init scripts for upstart and systemd are in the `init` directory.


Documentation
-------------

* Additional documentation in the [docs folder](docs).
