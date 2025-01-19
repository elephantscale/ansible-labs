# Lab: Using Advanced Conditionals in Ansible Playbooks

## Overview

In this lab, you will deepen your understanding of using advanced conditionals in Ansible playbooks. You will learn to combine multiple conditions with logical operators (`and`, `or`, `not`) and use the `| bool` filter to evaluate human-readable variables like "yes" or "no."

---

## Objectives

- Learn how to use advanced conditionals in tasks.
- Combine multiple conditions with logical operators.
- Use the `| bool` filter for evaluating variables.

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

1. Create a new YAML file named `advanced_conditional_playbook.yml`.

2. Define the play's name, target hosts (`all` in this case), and whether to gather facts:

   ```yaml
   ---
   - name: A conditional play with advanced logic
     hosts: all
     gather_facts: no
   ```

   **Explanation:**
   - `hosts: all` targets all hosts in the inventory.
   - `gather_facts: no` skips automatic fact gathering for simplicity.

---

### Step 2: Define Variables

1. In the `vars` section, define the following variables:

   ```yaml
     vars:
       epic: true
       boomer: no
       distro: "Ubuntu"
   ```

   **Explanation:**
   - `epic` is a boolean variable.
   - `boomer` is a human-readable string evaluated as a boolean using the `| bool` filter.
   - `distro` specifies the operating system.

---

### Step 3: Add Conditional Tasks

1. Under the `tasks` section, add the following tasks:

   - **Task A**: Executes if both `epic` is true and `boomer` evaluates to true.

     ```yaml
       - name: Task A
         debug:
           msg: "Hello A"
         when: epic and boomer | bool
     ```

   - **Task B**: Executes if `epic` is false and `boomer` evaluates to false.

     ```yaml
       - name: Task B
         debug:
           msg: "Hello B"
         when:
           - not epic
           - not boomer | bool
     ```

   - **Task C**: Executes if either `epic` is true or the `distro` is "Ubuntu."

     ```yaml
       - name: Task C
         debug:
           msg: "Hello C"
         when: epic or distro == "Ubuntu"
     ```

   - **Task D**: Executes if `epic` is true and either `boomer` evaluates to true or the `distro` is "Ubuntu."

     ```yaml
       - name: Task D
         debug:
           msg: "Hello D"
         when:
           - epic
           - boomer | bool or distro == "Ubuntu"
     ```

---

### Step 4: Execute the Playbook

1. Save the `advanced_conditional_playbook.yml` file.

2. Run the playbook using the following command:

   ```bash
   ansible-playbook advanced_conditional_playbook.yml
   ```

3. Observe the output. Each task will execute based on the specified conditions.

---

## Conclusion

In this lab, you've learned how to use advanced conditionals in Ansible playbooks by combining logical operators and boolean filters. These techniques allow you to create dynamic, flexible playbooks that respond intelligently to variable values and system states. Experiment with additional conditions to further refine your skills! 👏

