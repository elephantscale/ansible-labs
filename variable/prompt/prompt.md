# Lab: Using Prompts in Ansible Playbooks

## Overview

In this lab, you will learn how to use prompts in an Ansible playbook. Prompts allow you to request input from the user running the playbook, enabling customization without altering the playbook's code.

---

## Objectives

- Understand how to use `vars_prompt` to gather user input.
- Combine prompted and predefined variables in a task.
- Run a playbook and provide input through prompts.

---

## Prerequisites

- Basic understanding of YAML syntax.
- Ansible installed on your Ansible machine.
- Access to one or more hosts configured for management with Ansible.

---

## Duration

**Estimated Time:** 30 minutes

---

## Instructions

### Step 1: Create Your Playbook

1. On your Ansible machine, open a text editor and create a new file named `prompt_play.yml`.

2. Add the following content to the file:

    ```yaml
    ---
    - hosts: all
      name: A simple play with prompts
      gather_facts: no

      vars_prompt:

        - name: firstName
          prompt: Enter your first name
          private: no
          default: John

        - name: lastName
          prompt: Enter your last name
          private: no
          default: Doe

      vars:
        yourAge: 18

      tasks:
        - name: Display the input values
          debug:
            msg: "Hello {{ firstName }} {{ lastName }}, you are {{ yourAge }} years old."
    ```

    **Explanation:**
    - `vars_prompt`: Defines variables to be prompted from the user during playbook execution.
    - `vars`: Stores predefined static variables.
    - `tasks`: Executes actions, such as displaying messages that combine prompted and predefined variables.

---

### Step 2: Run Your Playbook

1. Save the `prompt_play.yml` file.

2. Execute the playbook using the following command:

    ```bash
    ansible-playbook prompt_play.yml
    ```

3. When prompted, enter a first name and last name, or press Enter to accept the default values (`John` and `Doe`).

4. Review the output. The message will include the entered names and the predefined age.

---

### Step 3: Experiment with Prompts

- Modify the default values in the `vars_prompt` section and rerun the playbook.
- Add new prompted variables and include them in tasks to explore further functionality.

---

## Conclusion

Congratulations! You have successfully completed a lab on using prompts in Ansible playbooks. This technique is essential for creating interactive playbooks that require user input. Keep exploring to become more proficient with Ansible's versatile features. 👏

