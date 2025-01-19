# Lab: Managing Complex Variables in Ansible Playbooks

## Overview

In this advanced lab, you will expand your skills in managing complex variables in Ansible. You'll work with multiple variable files, nested variables, and use these variables in various tasks. This approach is beneficial for managing complex configurations and scenarios in Ansible playbooks.

---

## Objectives

- Use multiple external variable files in a playbook.
- Define and utilize nested variables.
- Apply conditional statements based on variable values.
- Implement looping through variables.

---

## Prerequisites

- Intermediate understanding of YAML syntax.
- Ansible installed on your ansible machine.
- Access to one or more hosts configured for management with Ansible.

---

## Duration

**Estimated Time:** 45 minutes

---

## Instructions

### Step 1: Create Multiple Variable Files

1. **Create a General Variable File:**

    - Create a file named `general_vars.yml`.
    - Add general configuration variables. For example:

        ```yaml
        app_name: "MyApp"
        app_port: 8080
        ```

2. **Create an Environment-Specific Variable File:**

    - Create a file named `prod_vars.yml`.
    - Add production environment-specific variables. For example:

        ```yaml
        db_host: "prod-db.example.com"
        db_user: "prod_user"
        db_password: "prod_password"
        ```

---

### Step 2: Create Your Playbook with Complex Variables

1. Create a new file named `complex_var_play.yml`.

2. Add the following content to the file:

    ```yaml
    ---
    - hosts: servers
      gather_facts: no

      vars_files:
        - general_vars.yml
        - prod_vars.yml

      tasks:
        - name: Print general app information
          debug:
            msg: "App Name: {{ app_name }}, Port: {{ app_port }}"

        - name: Print database information in production
          debug:
            msg: "Production DB Host: {{ db_host }}, User: {{ db_user }}"
    ```

   **Explanation:**
    - `vars_files`: Includes both general and environment-specific variable files.
    - The `tasks` section demonstrates printing variables and accessing data from multiple files.

---

### Step 3: Run Your Playbook

1. Save the `complex_var_play.yml` file.

2. Execute the playbook using the following command:

    ```bash
    ansible-playbook complex_var_play.yml
    ```

3. Observe the output. The playbook will demonstrate the use of variables from different files.

---

### Step 4: Experiment with Advanced Variable Techniques

- Try adding nested variables (variables within variables) and accessing them in your tasks.
- Implement more complex `when` conditions using variable values.
- Experiment with loops using complex data structures like lists of dictionaries.

---

## Conclusion

Congratulations! You've successfully completed an advanced lab on managing complex variables in Ansible. These skills are crucial for handling sophisticated automation scenarios. Continue to explore and experiment with different variable management techniques to enhance your Ansible playbooks. 👏

