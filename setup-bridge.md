
# Setting up bridge adapter on host machine

```
cat /etc/netplan/50-cloud-init.yaml
sudo nano /etc/netplan/50-cloud-init.yaml
```

network:
  version: 2
  renderer: networkd

  ethernets:
    eth0:
              dhcp4: false
              dhcp6: false

  bridges:
    br0:
        interfaces: [eth0]
        dhcp4: true


# Setup node/add bridge adapter

```shell
lxc launch ubuntu:20.04 node1 && lxc launch ubuntu:20.04 node2 && lxc launch ubuntu:20.04 node3
lxc list
lxc config device add node1 eth0 nic nictype=bridged parent=br0 name=eth0
lxc config device add node2 eth0 nic nictype=bridged parent=br0 name=eth0
lxc config device add node3 eth0 nic nictype=bridged parent=br0 name=eth0
lxc list
lxc file push ~/.ssh/authorized_keys node1/root/.ssh/
lxc file push ~/.ssh/authorized_keys node2/root/.ssh/
lxc file push ~/.ssh/authorized_keys node3/root/.ssh/
ssh root@node1
```