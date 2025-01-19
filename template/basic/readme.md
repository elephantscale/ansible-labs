# Lab: Deploying a Configuration File using Templates in Ansible

## Overview

In this lab, participants will familiarize themselves with how Ansible uses the `template` module to deploy configuration files on servers. The lab emphasizes the importance of separating configuration data from tasks, allowing for dynamic content generation.

---

## Objectives

- Learn to use the `template` module for configuration file deployment.
- Understand how to use Jinja2 templates for dynamic value substitution.
- Create reusable playbooks for consistent configurations.

---

## Prerequisites

- Ansible installed on the participant's machine.
- A basic understanding of Ansible playbooks and templates.
- A target host or set of hosts categorized as `webservers` in your Ansible inventory.
- A template file named `config.ini.jn2` in your working directory.

---

## Duration

**Estimated Time:** 20 minutes

---

## Instructions

### Step 1: Set Up the Playbook

1. Create a new playbook named `config_deployment_demo.yml`.

2. Define the play structure:

   ```yaml
   ---
   - name: Deploy Configuration Files Using Templates
     hosts: webservers
     gather_facts: no
   ```

---

### Step 2: Define Play Variables

1. Add variables for database configuration under the `vars` section:

   ```yaml
     vars:
       dbUsername: root
       dbPassword: mySecurePassword
   ```

---

### Step 3: Create the Template File

1. Ensure you have a template file named `config.ini.jn2` in your working directory.

2. Add the following content to the template file:

   ```ini
   db_username = {{ dbUsername }}
   db_pass = {{ dbPassword }}
   ```

   **Explanation:**
   - `{{ dbUsername }}` and `{{ dbPassword }}` use Jinja2 syntax to dynamically substitute values from the playbook variables.

---

### Step 4: Deploy the Configuration File

1. Add a task to deploy the configuration file using the `template` module:

   ```yaml
     tasks:
       - name: Deploy the configuration file
         template:
           src: config.ini.jn2
           dest: ~/config.ini
   ```

   **Explanation:**
   - `src`: Specifies the source template file.
   - `dest`: Specifies the destination path on the target server.

---

### Step 5: Execute the Playbook

1. Save the `config_deployment_demo.yml` file.

2. Run the playbook using the following command:

   ```bash
   ansible-playbook config_deployment_demo.yml
   ```

3. Observe the output to ensure the configuration file is successfully deployed to the target servers.

---

## Solution File

You can compare your playbook with the [config_deployment_demo.yml](config_deployment_demo.yml) file in the current directory.

---

## Conclusion

This lab demonstrated how to use Ansible's `template` module to deploy dynamic configuration files. By utilizing templates, you can maintain consistent configurations across environments while allowing for dynamic substitutions. Mastering this technique is essential for creating scalable and adaptable automation workflows. 👏

