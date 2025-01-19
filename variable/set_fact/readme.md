# Lab: Using the `set_fact` Module in Ansible

## Overview

In this lab, you will learn how to use the `set_fact` module in Ansible to dynamically define custom facts (variables) during playbook execution and utilize them in subsequent tasks. This approach is
particularly useful for assigning or calculating values during runtime.

---

## Objectives

- Understand how to use the `set_fact` module.
- Dynamically define custom variables during playbook execution.
- Utilize the defined variables in subsequent tasks.

---

## Prerequisites

- Basic knowledge of Ansible playbooks and tasks.
- Ansible installed on your ansible machine.
- Access to target hosts in the `webservers` inventory group.

---

## Duration

**Estimated Time:** 20 minutes

---

## Instructions

### Step 1: Set Up the Ansible Playbook

1. Create a new YAML file named `facts_playbook.yml`.

2. Define the play with the following content:

   ```yaml
   ---
   - name: A play with custom variables (facts)
     hosts: webservers
     gather_facts: no
   ```

   **Explanation:**
    - `gather_facts: no` disables automatic fact-gathering, allowing you to define custom facts manually.

---

### Step 2: Define Facts Using the `set_fact` Module

1. Add tasks to define custom facts using the `set_fact` module. These facts can be static values or dynamically calculated.

   ```yaml
     tasks:
       - name: Setting custom facts
         set_fact:
           fact_one: "Hello"
           fact_other: "Bye"
           john_fact: "Doe"
   ```

   **Explanation:**
    - `fact_one`, `fact_other`, and `john_fact` are custom variables that will be available for the remainder of the playbook.

---

### Step 3: Utilize Facts in Subsequent Tasks

1. Use the defined facts in tasks. For example, use the `debug` module to print their values:

   ```yaml
       - name: Display john_fact
         debug:
           var: john_fact

       - name: Display fact_other
         debug:
           var: fact_other
   ```

   **Explanation:**
    - The `debug` module helps verify that the facts are set correctly and can display their values during playbook execution.

---

### Step 4: Execute the Playbook

1. Save the playbook as `facts_playbook.yml`.

2. Run the playbook using the following command:

   ```bash
   ansible-playbook facts_playbook.yml
   ```

3. Observe the output. The values of `john_fact` and `fact_other` will be displayed as defined in the `set_fact` module.

---

## Conclusion

In this lab, you’ve learned how to use the `set_fact` module to dynamically define and utilize custom variables during playbook execution. This powerful feature allows you to create flexible and
dynamic playbooks tailored to specific scenarios or conditions. Keep practicing with `set_fact` to enhance your Ansible automation skills! 👏

