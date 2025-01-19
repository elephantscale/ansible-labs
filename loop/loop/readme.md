# Lab: Iterating Over Complex Data Structures with Ansible

## Overview

In this lab, you will learn how to use Ansible's `loop` directive along with `loop_control` to iterate over a list of dictionaries. This method is more powerful and flexible than the older `with_items` directive. By the end of this lab, you'll have a clear understanding of how to handle complex loop scenarios using a custom loop variable.

---

## Objectives

- Use the `loop` directive to iterate over complex data structures.
- Leverage `loop_control` to customize variable names for better readability.
- Execute tasks dynamically based on dictionary values.

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

1. Create a new YAML file named `loop_complex_data.yml`.

2. Define the play's name, target hosts, and disable fact gathering:

   ```yaml
   ---
   - name: Iterate over a list of dictionaries
     hosts: webservers
     gather_facts: no
   ```

---

### Step 2: Add the Loop with Complex Data

1. Under the `tasks` section, add a task to iterate over a list of dictionaries, each containing IP and gateway details:

   ```yaml
     tasks:
       - name: Print network information
         loop_control: 
           loop_var: networkInfo
         debug:
           msg: "IP: {{ networkInfo.ip }}, Gateway: {{ networkInfo.gateway }}"
         loop:
           - { ip: "10.10.10.2", gateway: "10.10.10.1" }
           - { ip: "10.10.11.2", gateway: "10.10.11.1" }
           - { ip: "10.10.12.2", gateway: "10.10.12.1" }
   ```

   **Explanation:**
   - `loop`: Iterates over the provided list of dictionaries.
   - `loop_control`: Allows customization of the loop variable name, making the task more readable.
   - `{{ networkInfo.ip }}` and `{{ networkInfo.gateway }}`: Access dictionary values for each item in the loop.

---

### Step 3: Execute the Playbook

1. Save the `loop_complex_data.yml` file.

2. Run the playbook using the following command:

   ```bash
   ansible-playbook loop_complex_data.yml
   ```

3. Observe the output. For each dictionary in the list, the playbook will print the IP and gateway values.

---

## Solution File

You can compare your playbook with the [loop_complex_data.yml](loop_complex_data.yml) file in the current directory.

---

## Conclusion

Ansible's `loop` directive, especially when combined with `loop_control`, offers a robust mechanism for iterating over complex data structures. This method simplifies playbooks and improves readability when handling intricate looping scenarios. Practice applying this technique to enhance the flexibility and clarity of your Ansible automation tasks! 👏

