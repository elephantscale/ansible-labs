# Creating Your First Ansible Playbook

## Overview

In this lab, you will create and run your first Ansible playbook. A playbook is a YAML file where you define the tasks and settings for Ansible to execute on your hosts. You'll learn the basics of
playbook structure and how to perform simple tasks.

## Objectives

- Understand the structure of an Ansible playbook.
- Create a playbook with basic tasks.
- Run the playbook on a group of web servers.

## Prerequisites

- Basic knowledge of YAML syntax.
- Ansible installed on your control node (the machine where you run Ansible commands).
- Access to one or more hosts (like web servers) where you have SSH access and Ansible is configured to manage them.

## Duration

30 minutes

## Instructions

### Step 1: Create Your Playbook File

1. On your control node, open a text editor and create a new file named `first_play.yml`.

2. Add the following content to the file:

    ```yaml
    ---
    - name: Our First Play
      hosts: webservers
      gather_facts: no

      tasks:
        - name: Print something
          debug:
            msg: "Hello World"

        - name: Print something else
          debug:
            msg: "Good bye"
    ```

   Here's a breakdown of what each part does:
    - `---` signifies the start of a YAML file.
    - The first section defines the play:
        - `name`: A human-readable name for the play.
        - `hosts`: Specifies the group of hosts to target, in this case, `webservers`.
        - `gather_facts`: Set to `no` to skip gathering system facts (to speed up execution for this simple task).
    - Under `tasks`, we define what we want Ansible to do:
        - Each `- name` is a human-readable description of the task.
        - `debug` is a module provided by Ansible to print messages to the console.

### Step 2: Run Your Playbook

1. Save the `first_play.yml` file.

2. Run the playbook using the `ansible-playbook` command:

    ```bash
    ansible-playbook first_play.yml
    ```

3. Observe the output. Ansible will connect to the hosts in the `webservers` group and execute the tasks you defined. You should see "Hello World" and "Good bye" messages in the output.

### Step 3: Experiment and Explore

- Try modifying the messages in the `debug` tasks and rerun the playbook to see how the output changes.
- Experiment with targeting different host groups, if you have them configured.

## Conclusion

Congratulations! You've successfully created and run your first Ansible playbook. This foundational skill is a stepping stone to more complex Ansible automation tasks. As you become more comfortable
with playbooks, you'll be able to automate a wide range of tasks on your managed hosts.