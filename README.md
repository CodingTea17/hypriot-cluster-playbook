# Hypriot Cluster Playbook
A set of roles to configure and initialize a Kubernetes cluster on a group of Raspberry Pis running HypriotOS. There are two major components here. First off, we're bootstrapping the nodes using a cloud-init script (yes it's technically hacky because our init count >1...) and prepping them for duty. This step can involve changing the nodes' IPs and will fail when after it runs. To avoid that just use the DHCP assigned IPs and configure them as the static_host_ip. The second part involves using kubeadm to init a new cluster, fire up flannel, and then join the worker nodes.

# Setup
vars/variables.yml
```
gateway_ip: GATEWAY_IP (i.e. 172.25.1.1/24)
ssh_pub_key: PUBLIC_KEY_HERE (i.e. pbcopy < ~/.ssh/id_rsa.pub & paste here)
```

hosts.yml
```
# Existing Raspberry Pi DHCP IPs from nmap -sS -p 22 $NETWORK (i.e. 172.25.1.1/24)
[init-nodes]
172.25.1.150 host_name=master-node static_host_ip=172.25.1.50/24 
172.25.1.151 host_name=worker-node-01 static_host_ip=172.25.1.51/24
172.25.1.152 host_name=worker-node-02 static_host_ip=172.25.1.52/24
172.25.1.153 host_name=worker-node-03 static_host_ip=172.25.1.53/24
172.25.1.154 host_name=worker-node-04 static_host_ip=172.25.1.54/24
172.25.1.155 host_name=worker-node-05 static_host_ip=172.25.1.55/24
172.25.1.156 host_name=worker-node-06 static_host_ip=172.25.1.56/24
172.25.1.157 host_name=worker-node-07 static_host_ip=172.25.1.57/24

# Master node IPs
[masters]
172.25.1.50

# Worker node IPs
[workers]
172.25.1.51
172.25.1.52
172.25.1.53
172.25.1.54
172.25.1.55
172.25.1.56
172.25.1.57
```

execute `ansible-playbook configure-pis.yml -i hosts.yml` (if you haven't configured ssh login then add `--ask-pass`)