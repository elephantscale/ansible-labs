# Lab Instructions: Displaying CPU Count with Ansible

## Overview

In this beginner-friendly lab, you will learn how to use Ansible to gather facts about your managed hosts and display specific information. Specifically, you will create a playbook to display the number of CPUs on the target host.

## Objectives

- Understand how to gather facts in Ansible.
- Access and use gathered facts in tasks.
- Create a task to display the CPU count of a target host.

## Prerequisites

- Basic understanding of YAML syntax.
- Ansible installed on your control node.
- Access to one or more hosts configured for management with Ansible.

## Duration

20 minutes

## Instructions

### Step 1: Create Your Playbook

1. On your control node, open a text editor and create a new file named `cpu_count_playbook.yml`.

2. Start defining your playbook with the following content:

    ```yaml
    ---
    - hosts: servers
      gather_facts: yes
    ```

    - `hosts: servers` specifies that the playbook should run on hosts in the `servers` group.
    - `gather_facts: yes` tells Ansible to collect information about the host before executing tasks.

### Step 2: Add a Task to Display CPU Count

1. Under the `tasks` section, add a task to display the number of CPUs:

    ```yaml
      tasks:
        - name: Display CPU count
          debug:
            msg: "The target host has {{ ansible_processor_vcpus }} CPUs."
    ```

    - This task uses the `debug` module to print a message.
    - `{{ ansible_processor_vcpus }}` is a variable provided by Ansible's gathered facts, representing the number of CPUs on the host.

### Step 3: Run Your Playbook

1. Save the `cpu_count_playbook.yml` file.

2. Execute the playbook using the following command:

    ```bash
    ansible-playbook cpu_count_playbook.yml
    ```

3. Observe the output. The playbook will display the number of CPUs for each host in the `servers` group.

## Conclusion

Congratulations! You've successfully created an Ansible playbook that gathers facts about your hosts and displays the number of CPUs. This exercise demonstrates how to access and use the information collected by Ansible, a fundamental skill for more advanced automation tasks. Keep experimenting with different facts to explore what information you can gather and utilize.