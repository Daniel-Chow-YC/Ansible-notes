# Ansible Playbooks

## Playbooks
- An ansible playbook is the main way to automate tasks in ansible
- Playbooks are written in YAML text files
- Use ``ansible-playbook`` command to run a playbook
  - ``ansible-playbook <name_of_playbook>.yml``

### Simple Example
``ansible-playbook -i hosts -l host1.example.org apache.yml``

- ``apache.yml`` is the name of the playbook

## Handlers
- Handlers are tasks that respond to a notification triggered by other tasks
- Tasks only notify their handlers ``when the task changes something`` on a managed host
- If one or more tasks notify a handler, ``the handler will run only once``
- Handlers only run ``after all of the tasks are completed in a particular play``


## Loops
````
- hosts: all
  tasks:
    - name: Updates apt cache
      apt: update_cache=true

    - name: Installs necessary packages
      apt: pkg={{ item }} state=latest
      with_items:
        - apache2
        - libapache2-mod-php
        - git

````


## Using Conditionals
````
---
- hosts: web
  tasks:
    - name: Installs apache web server
      apt: pkg=apache2 state=present update_cache=true

    - name: Push future default virtual host configuration
      copy: src=files/awesome-app dest=/etc/apache2/sites-available/awesome-app.conf mode=0640

    - name: Deactivates the default virtualhost
      command: a2dissite 000-default

    - name: Activates our virtualhost
      command: a2ensite awesome-app

    - name: Check that our config is valid
      command: apache2ctl configtest
      register: result
      ignore_errors: True

    - name: Rolling back - Restoring old default virtualhost
      command: a2ensite 000-default
      when: result|failed

    - name: Rolling back - Removing our virtualhost
      command: a2dissite awesome-app
      when: result|failed

    - name: Rolling back - Ending playbook
      fail: msg="Configuration file is not valid. Please check that before re-running the playbook."
      when: result|failed

    - name: Deactivates the default ssl virtualhost
      command: a2dissite default-ssl

      notify:
        - restart apache

  handlers:
    - name: restart apache
      service: name=apache2 state=restarted
````

The ``register`` keyword records output from the ``apache2ctl configtest`` command (exit status, stdout, stderr, ...), and ``when: result|failed`` checks if the registered variable (``result``) contains a failed status.
