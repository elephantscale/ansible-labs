# Lab: Iterating Over External Variables with Ansible

## Overview

In this lab, you will learn how to use external variables with Ansible's `loop` directive. By leveraging external variables, playbooks can become more dynamic and reusable across different scenarios.

---

## Objectives

- Use the `loop` directive to iterate over variables defined in the playbook.
- Dynamically execute tasks based on external variable data.
- Enhance playbook flexibility for various environments.

---

## Prerequisites

- Basic knowledge of Ansible playbooks.
- Ansible installed on your ansible machine.
- Access to a target host categorized as "webservers."

---

## Duration

**Estimated Time:** 30 minutes

---

## Instructions

### Step 1: Set Up the Playbook Structure

1. Create a new YAML file named `loop_with_variables.yml`.

2. Define the play's name, target hosts, and disable fact gathering:

   ```yaml
   ---
   - name: Loop over external variables
     hosts: webservers
     gather_facts: no

     vars:
       user_list:
         - Alice
         - Bob
         - Charlie
   ```

   **Explanation:**
   - `vars`: Defines a list of users to iterate over.
   - `user_list`: Contains the names to be looped through.

---

### Step 2: Add the Loop

1. Under the `tasks` section, add a task to iterate over the `user_list` variable and print a custom message for each:

   ```yaml
     tasks:
       - name: Greeting each user
         debug:
           msg: "Hello, {{ item }}"
         loop: "{{ user_list }}"
   ```

   **Explanation:**
   - `loop`: Iterates over the values in the `user_list` variable.
   - `{{ item }}`: Refers to the current value in the loop iteration.

---

### Step 3: Execute the Playbook

1. Save the `loop_with_variables.yml` file.

2. Run the playbook using the following command:

   ```bash
   ansible-playbook loop_with_variables.yml
   ```

3. Observe the output. The playbook will print a greeting for each name in the `user_list` variable.

---

## Solution File

You can compare your playbook with the [loop_with_variables.yml](loop_with_variables.yml) file in the current directory.

---

## Conclusion

Using external variables with Ansible's `loop` directive enhances playbook flexibility and reusability. By defining variables externally, you can adapt playbooks for different environments or scenarios without modifying task logic. Practice using this technique to build dynamic and versatile Ansible playbooks! 👏

