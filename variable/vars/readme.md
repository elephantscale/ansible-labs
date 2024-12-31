# Lab Instructions: Introduction to Variables in Ansible Playbooks

## Overview

In this beginner-friendly lab, you will learn how to use variables in an Ansible playbook. Variables in Ansible are very similar to variables in any programming language. They are used to store values
that can be used and reused throughout your playbook. This lab will guide you through creating a simple playbook that uses variables.

## Objectives

- Understand the concept of variables in Ansible.
- Define and use variables in a playbook.
- Run the playbook to see how variables affect task execution.

## Prerequisites

- Basic understanding of YAML syntax.
- Ansible installed on your control node.
- Access to one or more hosts (like web servers) configured for management with Ansible.

## Duration

30 minutes

## Instructions

### Step 1: Create Your Playbook

1. On your control node, open a text editor and create a new file named `variable_play.yml`.

2. Add the following content to the file:

    ```yaml
    ---
    - hosts: webservers
      name: A play with variables
      gather_facts: no

      vars:
        first_name: John
        family: Doe

      tasks:
        - name: Print first name variable
          debug:
            var: first_name

        - name: Print family variable with message
          debug:
            msg: "Hello, {{ family }}"

        - name: Print both variables
          debug:
            msg: "{{ first_name }}, {{ family }}"
    ```

   Explanation:
    - `vars`: This section is used to define variables. Here, `first_name` and `family` are the two variables.
    - `tasks`: This section contains tasks that use the `debug` module to print variable values.

### Step 2: Run Your Playbook

1. Save the `variable_play.yml` file.

2. Execute the playbook using the following command:

    ```bash
    ansible-playbook variable_play.yml
    ```

3. Observe the output. You should see the values of `first_name` and `family` printed in different formats as specified in the tasks.

### Step 3: Experiment with Variables

- Try changing the values of `first_name` and `family` in the `vars` section and rerun the playbook to see how the output changes.
- Experiment with adding new variables and using them in tasks.

## Conclusion

Well done! You've completed a basic lab on using variables in Ansible playbooks. Understanding variables is crucial as they allow you to write more dynamic and reusable playbooks. Keep experimenting
with different types of variables and their applications in your Ansible projects.