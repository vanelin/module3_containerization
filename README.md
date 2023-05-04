#  task_3 (Container image з нуля)

### Utilites:
```bash
sudo apt install bridge-utils
sudo apt install iputils-ping
```
### Network Setup With runC Containers
```bash
brctl addbr box_network0
ip link set box_network0 up

ip addr add 10.10.10.1/24 dev box_network0
ip link add name veth-host type veth peer name veth-guest
ip link set veth-host up

brctl addif box_network0 veth-host
ip netns add box_network
ip link set veth-guest netns box_network
ip netns exec box_network ip link set veth-guest name eth1
ip netns exec box_network ip addr add 10.10.10.2/24 dev eth1
ip netns exec box_network ip link set eth1 up
ip netns exec box_network ip route add default via 10.10.10.1
```

### check
``` bash
sudo runc run demo
listening on [::]:8080

curl 10.10.10.2:8080
```