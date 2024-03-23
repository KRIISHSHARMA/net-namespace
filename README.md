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
- `ip link` (or [arp](https://www.baeldung.com/linux/arp-command)) inside netns will only show loopback interface and not eth0 or any other host network interfaces (successfully preventing the container from seeing host interface)

