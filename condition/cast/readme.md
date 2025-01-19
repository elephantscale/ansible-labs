# Lab: Using Boolean Filters in Ansible Playbooks

## Overview

In this lab, you will learn how to use boolean filters in Ansible playbooks. Specifically, you will understand how the `| bool` filter can evaluate human-friendly variable values like "yes" or "no" into boolean `true` or `false`, and use them to control task execution.

---

## Objectives

- Learn how to use the `| bool` filter to evaluate variables.
- Create tasks that execute conditionally based on boolean filters.

---

## Prerequisites

- Basic understanding of Ansible playbooks.
- Ansible installed on your ansible machine.
- Access to target hosts in your inventory.

---

## Duration

**Estimated Time:** 20 minutes

---

## Instructions

### Step 1: Set Up the Ansible Playbook

1. Create a new YAML file named `conditional_bool_playbook.yml`.

2. Define the play's name, target hosts (`all` in this case), and whether to gather facts:

   ```yaml
   ---
   - name: A conditional play with boolean filters
     hosts: all
     gather_facts: no
   ```

   **Explanation:**
   - `hosts: all` targets all hosts in the inventory.
   - `gather_facts: no` skips automatic fact gathering for simplicity.

---

### Step 2: Define a Variable

1. In the `vars` section, define a variable named `boomer` and assign it the value `no`:

   ```yaml
     vars:
       boomer: no # "no" translates to false, "yes" translates to true
   ```

   **Explanation:**
   - The variable `boomer` is a human-readable string that will be evaluated as a boolean.

---

### Step 3: Add a Conditional Task

1. Under the `tasks` section, add a task that prints a message "Hello A" if the `boomer` variable evaluates to `true`:

   ```yaml
     tasks:
       - name: Task A
         debug:
           msg: "Hello A"
         when: boomer | bool
   ```

   **Explanation:**
   - The `when: boomer | bool` clause ensures the task runs only if `boomer` evaluates to `true` (e.g., "yes").

---

### Step 4: Execute the Playbook

1. Save the `conditional_bool_playbook.yml` file.

2. Run the playbook using the following command:

   ```bash
   ansible-playbook conditional_bool_playbook.yml
   ```

3. Observe the output. The task "Task A" will execute only if `boomer` evaluates to `true`.

---

## Conclusion

In this lab, you've learned how to use boolean filters in Ansible playbooks to evaluate variables like "yes" or "no" into boolean values. By leveraging the `| bool` filter, you can make your playbooks more adaptable and intuitive. Experiment with other human-friendly values and conditions to further enhance your automation skills! 👏

