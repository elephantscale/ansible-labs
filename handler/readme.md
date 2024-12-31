# Installing Apache2 with Handlers in Ansible

## Duration

Approximately 30 minutes

## Objective

In this lab, you will learn how to use Ansible handlers to respond to change events in your tasks. Specifically, you'll be installing the `apache2` package and, upon its successful installation, triggering handlers to display success messages.

## Prerequisites:

- Ansible installed on your machine.
- A target host or set of hosts configured in your Ansible inventory under the group `webserver`.
- The target host should have `apt` package manager (typically an Ubuntu/Debian machine).

## Step-by-step guide:

### Step A: Setting up the Playbook

- Start by creating a new playbook named `apache_install_handlers.yml`.
- Define the playbook structure:

```yaml
---
- hosts: webserver
  name: Install Apache2 with Handlers
  become: yes
  become_user: root
  gather_facts: no
```

### Step B: Installing Apache2 Package

- Add the task that installs the Apache2 package using the `apt` module:

```yaml
  tasks:
    - name: update package
      apt: 
        update_cache: yes
      
    - name: Install Package
      apt:
        name: apache2
      notify:
        - sucs_msg
        - sucs_msg_b
```

This task instructs Ansible to install the `apache2` package. If the package installation changes the state of the machine (e.g., installing or updating the package), the `notify` directive will trigger the named handlers.

### Step C: Define Handlers for Post-installation Messages

- Beneath the tasks, define handlers that display success messages:

```yaml
  handlers:
    - name: sucs_msg
      debug:
        msg: Package has been Installed

    - name: sucs_msg_b
      debug:
        msg: Package has been Installed B
```

Handlers are special types of tasks in Ansible that run only if notified by another task. In our playbook, upon successful installation of the Apache2 package, both handlers will be triggered, displaying their respective success messages.

### Step D: Running the Playbook

- Save the `apache_install_handlers.yml` file.
- Execute the playbook using the following command:

```bash
ansible-playbook apache_install_handlers.yml
```

Watch as Ansible installs the Apache2 package on the target `webserver` host(s). If the package gets installed or updated, you'll see the success messages from the handlers.

## Final File

You can compare your playbook with the [apache_install_handlers.yml](apache_install_handlers.yml) file in the current directory.

## Summary

Handlers provide a powerful way to execute tasks in response to changes detected by other tasks. They are useful for cases where you want to restart a service after its configuration changes or, like in this lab, provide feedback once a certain task has completed successfully. Mastering handlers can
help you create more efficient and responsive playbooks.