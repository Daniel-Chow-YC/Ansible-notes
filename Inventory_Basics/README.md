# Inventory Basics

## Location
- ``ansible --version`` will show which config file is in use
  - That config file will specify the location of the inventory
- The default place for the inventory file is ``/etc/ansible/hosts``.
- However, you can configure ansible to look somewhere else, use an environment variable (``ANSIBLE_HOSTS``), or use the ``-i`` flag in ansible commands and provide the inventory path.

## Grouping
Hosts in inventory can be grouped arbitrarily. For instance, you could have a debian group, a web-servers group, a production group, etc...
````
[debian]
host0.example.org
host1.example.org
host2.example.org
````
This can even be expressed shorter:
````
[debian]
host[0:2].example.org
````
If you wish to use child groups, just define a [groupname:children] and add child groups in it. For instance, let's say we have various flavors of linux running, we could organize our inventory like this:
````
[ubuntu]
host0.example.org

[debian]
host[1:2].example.org

[linux:children]
ubuntu
debian
````

## Setting Variables in Inventory
````
[ubuntu]
host0.example.org ansible_host=192.168.0.12 ansible_port=2222
````
- ansible_host sets the IP address for the host
- ansible_port has the same function regarding the ssh port ansible will try to connect on.
