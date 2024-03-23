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
