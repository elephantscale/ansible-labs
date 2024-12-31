# Lab Instructions: Using Prompts in Ansible Playbooks

## Overview

In this beginner-friendly lab, you will learn how to use prompts in an Ansible playbook. Prompts are a way to ask for input from the user running the playbook. This can be useful for customizing playbook runs without changing the playbook's code.

## Objectives

- Learn to use `vars_prompt` to gather user input.
- Combine prompted variables and predefined variables in a task.
- Run the playbook and provide input through prompts.

## Prerequisites

- Basic understanding of YAML syntax.
- Ansible installed on your control node.
- Access to one or more hosts configured for management with Ansible.

## Duration

30 minutes

## Instructions

### Step 1: Create Your Playbook

1. On your control node, open a text editor and create a new file named `prompt_play.yml`.

2. Add the following content to the file:

    ```yaml
    ---
    - hosts: all
      name: A simple play with prompt
      gather_facts: no

      vars_prompt:

        - name: firstName
          prompt: Enter Your First Name
          private: no
          default: John

        - name: lastName
          prompt: Enter lastName
          private: no
          default: Doe

      vars:
        yourAge: 18

      tasks:
        - name: Print values
          debug:
            msg: "Hello {{ firstName }} {{ lastName }} you are {{ yourAge }} years old"
    ```

    Explanation:
    - `vars_prompt`: This section is used to define variables that will be prompted to the user when the playbook is run.
    - `vars`: This section is used to define static variables.
    - `tasks`: Contains tasks that use the `debug` module to print a message combining prompted and static variables.

### Step 2: Run Your Playbook

1. Save the `prompt_play.yml` file.

2. Execute the playbook using the following command:

    ```bash
    ansible-playbook prompt_play.yml
    ```

3. When prompted, enter a first name and last name. You can also press Enter to accept the default values (`John` and `Doe`).

4. Observe the output. The message will include the names you entered and the predefined age.

### Step 3: Experiment with Prompts

- Try modifying the default values in the `vars_prompt` section and rerun the playbook.
- Experiment with adding new prompted variables and using them in tasks.

## Conclusion

Congratulations! You've successfully completed a lab on using prompts in Ansible playbooks. This skill is useful for creating interactive playbooks where user input is required. Keep practicing to become more proficient with Ansible's various features.