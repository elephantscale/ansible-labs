# Lab: Include Tasks in Ansible

## Overview

In this lab, you'll learn how to use `include_tasks` in Ansible to include separate task files without using any conditions. This basic example will help you understand how to split tasks across multiple files and include them in your playbook.

---

## Duration

**Estimated Time:** 20 minutes

---

## Objectives

- Use `include_tasks` to include task files in a playbook.

---

## Prerequisites

- Ansible installed on your ansible machine.
- At least one managed host.

---

## Instructions

### Step 1: Create Task Files

1. **Create a Task File Named `common_tasks.yml`:**

   - Create a file named `common_tasks.yml` with the following content:

     ```yaml
     ---
     - name: Debug message for common tasks
       debug:
         msg: "Running common tasks for all environments"
     ```

2. **Create a Task File Named `env_specific_tasks.yml`:**

   - Create a file named `env_specific_tasks.yml` with the following content:

     ```yaml
     ---
     - name: Debug message for environment-specific tasks
       debug:
         msg: "Running environment-specific tasks"
     ```

---

### Step 2: Create the Main Playbook

1. Create a playbook file named `include_tasks_play.yml` with the following content:

   ```yaml
   ---
   - name: Include task files example
     hosts: all
     gather_facts: no

     tasks:
       - name: Include common tasks
         include_tasks: common_tasks.yml

       - name: Include environment-specific tasks
         include_tasks: env_specific_tasks.yml
   ```

---

### Step 3: Run the Playbook

1. Save the playbook and task files.

2. Run the playbook using the following command:

   ```bash
   ansible-playbook include_tasks_play.yml
   ```

3. Observe the output. The playbook will include and execute both task files, displaying debug messages from `common_tasks.yml` and `env_specific_tasks.yml`.

---

## Conclusion

In this simple lab, you've learned how to use `include_tasks` to load multiple task files into an Ansible playbook. This method allows you to organize tasks into separate files and reuse them in different playbooks, improving modularity and maintainability. 👏

