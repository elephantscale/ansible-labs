<link rel='stylesheet' href='assets/css/main.css'/>

# Ansible Labs

![](https://upload.wikimedia.org/wikipedia/commons/thumb/2/24/Ansible_logo.svg/200px-Ansible_logo.svg.png)

## Lab Environment

Instructor will provide access to lab environment

## Note

In the labs term `C&C` means `Control Center`

## Requirements

- All these labs are for ansible, your instructor will review UNIX commands with you before working on ansible
- Ansible uses `YAML` files, your instructor will cover and talk about how to create a yaml file with you
- At the time of writing this lab, there are over 3000 modules, we will only cover some of them as an example.

## Setup Labs

### Install and Setup

* [Setup ansible control center](setup/Install-Ansible.md)
* [Setup inventory](setup/Configure-Inventory.md)


### Modules

* [Ping Hosts](modules/Module-Ping.md)
* [Facts](modules/Module-Facts.md)
* [File/folder manipulations](modules/Module-File.md)
* [APT](modules/Module-apt.md)

### Jinja2

* [Jinja2](jinja2/sample.yml)


### PlayBook

* [Variable](playbook/variable/sample.yml)
* [Loop](playbook/loop/sample.yml)
* [Condition](playbook/condition/sample.yml)
* [Handler](playbook/handler/sample.yml)
* __Sample__: [Apache2](playbook/apache2)
* __Sample__: [LAMP](playbook/lamp/sample.yml)

### Extra

* [Log per Host](extra/log_per_host/README.md)


### Sample

* __Sample__: [LAMP Role](role/lamp/site.yml)
* __Sample__: [Wordpress Role](role/wordpress/sites.yml)


### YAML

* [Writing YAML file](yaml/README.md)