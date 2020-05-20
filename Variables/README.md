# Variables

## Calling Variables
- A variable can be called/referenced by placing the variable name in double braces: ``{{variable_name}}``
- When calling one variable as another variable's value, you must use ``quotes`` around the value.

````
- name: example play
  hosts: all
  vars:
    user_name: joe

  tasks:
  name: create the user {{user_name}}
  user:
    name: "{{user_name}}"
    state: present

````

## Group and host variable files
Ansible will look for variables definitions in group and host variable files.

For example:
- group_vars/webservers
- group_vars/dbservers
- host_vars/host0.example.org

- Host variables apply to a specific host
- Group variables apply to all hosts in that host group

## Setting Vatiables in Inventory
````
[webservers]
host0.example.org ansible_host=192.168.0.12 ansible_port=2222
````
- ansible_host sets the IP address for the host
- ansible_port has the same function regarding the ssh port ansible will try to connect on.
