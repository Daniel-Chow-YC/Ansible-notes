# Ad Hoc Commands

##  Common Flags
- ``-m`` the module being used
- ``-i`` path to inventory
- ``-a`` to pass arguements
- ``-l`` limit to specific host from inventory

## Examples

### Ping module
``ansible -m ping all -i hosts``

### Service module
``ansible -i hosts -m service -a 'name=apache2 state=restarted' host1.example.org``

### Shell module
``ansible -i hosts -m shell -a 'grep DISTRIB_RELEASE /etc/lsb-release' all``

 ``all`` is a shortcut meaning ``all hosts found in inventory file``

### Copy module
``ansible -i hosts -m copy -a 'src=/etc/motd dest=/tmp/' host0.example.org``

With this module you can copy a file from the controlling machine to the node.

### Setup module
``ansible -i hosts -m setup -a 'filter=ansible_memtotal_mb' all``

The setup module specializes in node's facts gathering -- getting information aboyt the node.

When using the setup module, you can use ``*`` in the ``filter=`` expression.

## Selecting hosts

We saw that all means 'all hosts', but ansible provides a lot of other ways to select hosts: http://docs.ansible.com/ansible/latest/intro_patterns.html

- ``host0.example.org:host1.example.org`` would run on host0.example.org and host1.example.org
- ``host*.example.org`` would run on all hosts starting with 'host' and ending with '.example.org'
