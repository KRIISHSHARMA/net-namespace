# net-namespace
isolation of the host network stack

# What are namespaces?
They provide processes with their own system view, thus isolating independent processes from each other.

# Types of namespaces
- PID namespace: isolation of the system process tree;
- NET namespace: isolation of the host network stack;
- MNT namespace: isolation of host filesystem mount points;
- UTS namespace: isolation of hostname;
- IPC namespace: isolation for interprocess communication utilities (shared segments, semaphores);
- USER namespace: isolation of system users IDs;
- GROUP namespace: isolation of the virtual cgroup filesystem of the host.

container has its own arp and routing tables , cant see hosts but host can see its processes (`ps aux`) 

# To create a new network NS on linux host
``` sh
ip netns add <name of nets ns>
```
# To see netns 
``` sh
ip netns
```
![image](https://github.com/KRIISHSHARMA/net-namespace/assets/86760658/aa4da807-3460-404a-ac68-4a2859a045de)

# To execute cmd inside ns 
``` sh
ip netns exec <name of ns> <cmd>
ip netns exec red ip link
ip -n red ip link
```
- `ip link` (or [arp](https://www.baeldung.com/linux/arp-command) , `route`) inside netns will only show loopback interface and not eth0 or any other host network interfaces (successfully preventing the container from seeing host interface)
-  [arp](https://www.baeldung.com/linux/arp-command) : The Address Resolution Protocol (ARP) handles the mapping between an Internet Protocol (IP) address and a Media Access Control (MAC) address. Linux stores all such mappings in a local system cache called an ARP table.

# Connecting ns with veth/virt cable
- To create the cable (specify the 2 ends veth-red and veth-blue
``` sh
ip link add veth-red type veth peer name veth-blue
```
![image](https://github.com/KRIISHSHARMA/net-namespace/assets/86760658/1308ae33-4b36-4e96-926a-80465b1291b7)

- Now attach each interafce with appropriate ns
``` sh
ip link set veth-red netns red
ip link set veth-blue netns blue
```
![image](https://github.com/KRIISHSHARMA/net-namespace/assets/86760658/461c32c3-9307-430a-9a4f-c2508edbfaae)

- Assign ip within each ns
```sh
ip -n red addr add 192.168.15.1 dev veth-red
ip -n blue addr add 192.168.15.2 dev veth-blue
```
![image](https://github.com/KRIISHSHARMA/net-namespace/assets/86760658/9cb0425a-bf19-44a1-af88-388552d399b9)

- Bringing up the interface using `set up` cmd within respective ns
``` sh
ip -n red link set veth-red up
ip -n blue link set veth-blue up
```
- To see if it worked , trying to ping from red ns to the ip of blue
``` sh
