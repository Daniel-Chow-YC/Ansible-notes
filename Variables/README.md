# Variables

## Group and host variable files
Ansible will look for variables definitions in group and host variable files.

For example:
- group_vars/linux
- group_vars/ubuntu
- host_vars/host0.example.org



## Setting Vatiables in Inventory
````
[ubuntu]
host0.example.org ansible_host=192.168.0.12 ansible_port=2222
````
- ansible_host sets the IP address for the host
- ansible_port has the same function regarding the ssh port ansible will try to connect on.
