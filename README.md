# tinc-cluster

Automatically setup a Tinc Meshed Network

This is a script that automatically installs [tinc](http://www.tinc-vpn.org) and setup the needed configuration to setup a meshed server-less network.

I only support Ubuntu but pull requests to add support for more distributions are happily accepted (it should be really easy do do so).

I created this as a easy way to setup a isolated network between several servers on the internet and I use this with my Docker deployment script [shdeploy](https://github.com/nsg/shdeploy) for a shared network.

## How to use

The script reads a white space separated list (separated by a space, tab, newline, ...) of DNS addresses (or raw IP if you prefer). There are two parameters where only the first one are required.

```
usage: ./setup-vpn name
       name     Name the VPN/device, keep it to [a-z0-9].
       id       Select 0-254, the network addresses will be
                based on 10.10.{id}.{incremental number}.
                Leve this empty to use the default 0 value.
       STDIN    Send a list of hosts to connect to on STDIN.
```

## Example

Create a shared network called `vpn` with the adresses 10.10.0.1 and 10.10.0.2. The addresses are automatically assigned
and they can change if you run this script again.

```
echo host1.example.com host2.example.com | ./setup-vpn vpn
```

The script will open plenty of ssh connections against the servers as root, ssh keys is probably a good idea. This script is intended to be running on your local computer where you have all the keys.

Note 1: If you run this again, the VPN will go down and all keys will be regenerated. The VPN will be started again when everything is done.

Note 2: Is 10.10.0.0/24 already in use? Change the network id in the second parameter. For example to add a second network run.

```
echo host8.example.com host2.example.com | ./setup-vpn vpn2 2
```
