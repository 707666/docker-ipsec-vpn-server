# Docker IPsec/L2TP and Cisco IPsec VPN Server

[![Docker Pulls](https://img.shields.io/docker/pulls/hwdsl2/ipsec-vpn-server.svg)](https://hub.docker.com/r/hwdsl2/ipsec-vpn-server) 
[![Docker Stars](https://img.shields.io/docker/stars/hwdsl2/ipsec-vpn-server.svg)](https://hub.docker.com/r/hwdsl2/ipsec-vpn-server)

Use this image to set up your own VPN server supporting both `IPsec/L2TP` and `IPsec/XAuth ("Cisco IPsec")`.

Available on [Docker Hub](https://hub.docker.com/r/hwdsl2/ipsec-vpn-server/). Based on Debian Jessie with [Libreswan](https://libreswan.org) (IPsec VPN software) and [xl2tpd](https://github.com/xelerance/xl2tpd) (L2TP daemon).

## Download

```
docker pull hwdsl2/ipsec-vpn-server
```

## How to use this image

### Environment variables

This Docker image uses the following three environment variables, that can be declared in an `env` file:

```
VPN_IPSEC_PSK=<IPsec pre-shared key>
VPN_USER=<VPN Username>
VPN_PASSWORD=<VPN Password>
```

This will create a single account for VPN login. The IPsec PSK (pre-shared key) is specified by the `VPN_IPSEC_PSK` environment variable. The VPN username is defined in `VPN_USER`, and VPN password is specified by `VPN_PASSWORD`.

All the variables to this image are optional, which means you don't have to type in any environment variables, and you can have an IPsec VPN server out of the box! Read the sections below for details.

**Note:** In your `env` file, do not put single or double quotes around values, and do not add space around `=`.

### Start the IPsec VPN server

First, run `modprobe af_key` on the host to load the IPsec `NETKEY` kernel module.

Start your Docker container with the following command (replace `./vpn.env` with your own `env` file) :

```
docker run \
    --name docker-ipsec-vpn-server \
    --env-file ./vpn.env \
    -p 500:500/udp \
    -p 4500:4500/udp \
    -v /lib/modules:/lib/modules:ro \
    -d --privileged \
    hwdsl2/ipsec-vpn-server
```

### Retrieve VPN login details

If you did not set environment variables via an `env` file, `VPN_USER` will default to `vpnuser` and both `VPN_IPSEC_PSK` and `VPN_PASSWORD` will be randomly generated. To retrieve them, show the logs of the running container:

```
docker logs docker-ipsec-vpn-server
```

Search for these lines in the output:

```console
Connect to your new VPN with these details:

Server IP: <VPN Server IP>
IPsec PSK: <IPsec pre-shared key>
Username: vpnuser
Password: <VPN Password>
```

### Check server status

To check the status of your IPsec VPN server, you can pass `ipsec status` to your container like this :

```
docker exec -it docker-ipsec-vpn-server ipsec status
```

## Next Steps

Get your computer or device to use the VPN. Please refer to:

[Configure IPsec/L2TP VPN Clients](https://github.com/hwdsl2/setup-ipsec-vpn/blob/master/docs/clients.md)   
[Configure IPsec/XAuth VPN Clients](https://github.com/hwdsl2/setup-ipsec-vpn/blob/master/docs/clients-xauth.md)

Enjoy your very own VPN! :sparkles::tada::rocket::sparkles:

## Services Running

There are two services running: `Libreswan (pluto)` for the IPsec VPN, and `xl2tpd` for L2TP support.

The default IPsec configuration supports:

* IKEv1 with PSK and XAuth ("Cisco IPSec")
* IPsec/L2TP with PSK

The ports that are exposed for this container to work are:

* 4500/udp and 500/udp for IPsec

## See Also

* [hwdsl2/setup-ipsec-vpn](https://github.com/hwdsl2/setup-ipsec-vpn)

## License

Copyright (C) 2016&nbsp;Lin Song&nbsp;&nbsp;&nbsp;<a href="https://www.linkedin.com/in/linsongui" target="_blank"><img src="https://static.licdn.com/scds/common/u/img/webpromo/btn_viewmy_160x25.png" width="160" height="25" border="0" alt="View my profile on LinkedIn"></a>    
Based on [the work of Thomas Sarlandie](https://github.com/sarfata/voodooprivacy) (Copyright 2012)

This work is licensed under the [Creative Commons Attribution-ShareAlike 3.0 Unported License](http://creativecommons.org/licenses/by-sa/3.0/)   
Attribution required: please include my name in any derivative and let me know how you have improved it!