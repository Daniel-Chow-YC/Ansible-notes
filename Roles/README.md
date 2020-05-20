# Roles

- Ansible roles allow you to make automation code more reusable
- Roles provide pre-packaged tasks that can be configured through variables
- Allows youto create generic code for one project and reuse it on other projects

## Role Directory Structure
The command ``ansible-galaxy init <name_of_role>`` can create the directory structure for you.

[![ansible-roles.png](https://i.postimg.cc/ht55vCmF/ansible-roles.png)](https://postimg.cc/FY0ZnVGG)

- where ``role_example`` is the name of the role
- For example you would move the tasks from a playbook to ``role_example/tasks/main.yml`` and you would move the handlers from a playbook to ``role_example/handlers/main.yml`` etc...

## Using a Role in a Playbook

````
- name: play to create a shared folder
  hosts: all

  roles:
    - vsftpd_server
````
- This plays calls a role called ``vsftpd_server``
- There are no tasks in this play, however playbooks can have both tasks and roles.
- ``Roles will always run before tasks``, even when tasks are defined first.

````
- name: play to create a shared folder
  hosts: all

  roles:
    - vsftpd_server

    - role: vsftpd_server
      vars:
        ftp_config_src: vsftpd_special.conf.j2
````
- In this example the role is called twice
- The first time it iscalled with its default options
- The second time it overides the role's default variables and uses a different Jinja2 template for its config file.

## Ansible Galaxy
Ansible Galaxy refers to the Galaxy website where users can share roles, and to a command line tool for installing, creating and managing roles.
