# Ansible Labs

![](https://upload.wikimedia.org/wikipedia/commons/thumb/2/24/Ansible_logo.svg/200px-Ansible_logo.svg.png)

## Lab Environment

Ubuntu 22.04 LTS workstation with Ansible installed.

## Requirements

- All these labs are for ansible, your instructor will review UNIX commands with you before working on ansible
- Ansible uses `YAML` files, your instructor will cover and talk about how to create a yaml file with you
- At the time of writing this lab, there are over 3000 modules; we will only cover some of them as an example.

## Labs

### 1. Installation and Setup

- 1.1 [Install Ansible](setup/Install-Ansible.md)
- 1.2 [Setup Inventory](setup/Configure-Inventory.md)

### 2. AdHoc

- 2.1 [Ping Hosts](adhoc/ping.md)
- 2.2 [Facts](adhoc/facts.md)
- 2.3 [File/folder manipulations](adhoc/file.md)

### 3. Variables

- [Basic Playbook](basic/readme.md)
- [Variable in Playbook](variable/vars/readme.md)
- [Prompt the user](variable/prompt/prompt.md)
- [Variables in external](variable/external/readme.md)
- [Include Variables](variable/include_vars/readme.md)
- [Register Variable](variable/register/readme.md)
- [Set Fact](variable/set_fact/readme.md)
- [Get Facts](extra/cpu_count/readme.md)

### 4. Condition

- [Basic](condition/basic/readme.md)
- [Cast variables](condition/cast/readme.md)
- [Complex](condition/complex/readme.md)
- [Assert](condition/assert/readme.md)
- [Challenge](condition/challenge/readme.md)

### 5. Loops

- [With_items](loop/with_items/readme.md)
- [Loop](loop/loop/readme.md)
- [Loop with external variable](loop/loop_var/readme.md)
- [Loop external task file](loop/loop_task/readme.md)

### 6. Handler

- [Handler](handler/readme.md)

### 7. Blocks

- [Block](block/simple/readme.md)

### 8. Templates

- [Template](template/basic/readme.md)

### 9. Ansible Vault

- [Vault](vault/readme.md)

### 10. Roles

- [Roles](role/readme.md)

### Extra

- [Log per host](extra/log_per_host/readme.md)
- [Fact cache](extra/fact_cache/readme.md)1
- [Include Tasks](extra/include_tasks/readme.md)

