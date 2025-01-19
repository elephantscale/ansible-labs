# Lab: Iterating Over Items with Ansible

## Overview

In this lab, you will learn how to use the `with_items` directive in Ansible to iterate over a list of items and execute tasks for each item. By the end, you'll be familiar with iterating over a list in a playbook and displaying a customized message for each list item.

---

## Objectives

- Use the `with_items` directive to iterate over lists.
- Execute tasks dynamically for each item in a list.

---

## Prerequisites

- Basic knowledge of Ansible playbooks.
- Ansible installed on your ansible machine.
- Access to a target host categorized as "webservers."

---

## Duration

**Estimated Time:** 20 minutes

---

## Instructions

### Step 1: Set Up the Playbook Structure

1. Create a new YAML file named `item_iteration.yml`.

2. Define the play's name, target hosts, and enable privilege escalation:

   ```yaml
   ---
   - name: Iterate over a list of items
     hosts: webservers
     become: yes
   ```

---

### Step 2: Add the Iteration Task

1. Under the `tasks` section, add a task to iterate over a list of names and print a custom message for each:

   ```yaml
     tasks:
       - name: Print a message for each item
         debug:
           msg: "Hello, {{ item }}"
         with_items:
           - John
           - Jane
           - Alex
           - Sarah
   ```

   **Explanation:**
   - `with_items`: Iterates over the provided list.
   - `{{ item }}`: Refers to the current item in the iteration.

---

### Step 3: Execute the Playbook

1. Save the `item_iteration.yml` file.

2. Run the playbook using the following command:

   ```bash
   ansible-playbook item_iteration.yml
   ```

3. Observe the output. The playbook will print a custom message for each name in the list.

---

## Solution File

You can compare your playbook with the [item_iteration.yml](item_iteration.yml) file in the current directory.

---

## Conclusion

In this lab, you learned how to use the `with_items` directive in Ansible to iterate over lists and execute tasks multiple times with different data. This technique is valuable for automating repetitive operations and making playbooks more dynamic and efficient. Practice applying list iteration to various tasks in your Ansible playbooks to enhance your automation capabilities! 👏

