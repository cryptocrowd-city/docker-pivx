# PIVX wallet for docker

Docker image that runs the pivx daemon which can be turned into a masternode with the correct configuration.

If you like this image, buy me a coffee ;) BTC tip address: `bc1qrepygjyxq34d08dqpwhryej5aclpqgzc28seqt`

## Quick Start (daemon/masternode)

```text
docker run \
  -d \
  -v /home/username/crypto/pivx:/pivx \
  -p 51472:51472 \
  --name=pivx \
  guggero/pivx
```

for docker-compose:

```text
docker-compose up -d pivx
```

This will create the folder `.pivx` in `/home/username/crypto/pivx` with a bare `pivx.conf`.
You might want to edit the `pivx.conf` before running the container
because with the bare config file it doesn't do much, it's basically just an empty wallet.

Start as masternode
------------

In general, you should follow a tutorial on how to prepare your masternode (create private key, send PIVX and so on).
I suggest [this tutorial](https://pivxmasternode.org/2017/03/08/step-step-guide-setting-masternode/).

To start the masternode functionality, edit your pivx.conf (should be in /home/username/crypto/pivx/.pivx/
following the docker run command example above):

```
rpcuser=<SOME LONG RANDOM USER NAME>
rpcpassword=<SOME LONG RANDOM PASSWORD>
rpcallowip=::/0
server=1
logtimestamps=1
maxconnections=256
printtoconsole=1
masternode=1
masternodeaddr=<SERVER IP ADDRESS>:51472
masternodeprivkey=<MASTERNODE PRIVATE KEY>
```

Where `<SERVER IP ADDRESS>` is the public facing IPv4 or IPv6 address that the masternode will be reachable at.
Don't forget to put your IPv6 address in brackets! For example `[aaaa:bbbb:cccc::0]:51472`

`<MASTERNODE PRIVATE KEY>` is the private key that you generated earlier (with `pivx-cli masternode genkey`).

## Run QT GUI

```text

XSOCK=/tmp/.X11-unix
XAUTH=/tmp/.docker-xauth

xauth nlist ${DISPLAY} | sed -e 's/^..../ffff/' | xauth -f ${XAUTH} nmerge -
docker run \
  -d \
  -e "XAUTHORITY=${XAUTH}" \
  -e DISPLAY=$DISPLAY \
  -v ${XAUTH}:${XAUTH} \
  -v ${XSOCK}:${XSOCK} \
  -v /home/username/crypto/pivx:/pivx \
  -p 51472:51472 \
  --name=pivx \
  guggero/pivx /opt/pivx/bin/pivx-qt
```

for docker-compose:

```text
docker-compose up -d pivx-qt
```
