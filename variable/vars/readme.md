# Lab: Introduction to Variables in Ansible Playbooks

## Overview

In this beginner-friendly lab, you will learn how to use variables in an Ansible playbook. 

Variables in Ansible are very similar to variables in any programming language. They are used to store values that can be used and reused throughout your playbook. This lab will guide you through creating a simple playbook that uses variables.

---

## Objectives

- Understand the concept of variables in Ansible.
- Define and use variables in a playbook.
- Run the playbook to see how variables affect task execution.

---

## Prerequisites

- Basic understanding of YAML syntax.
- Ansible installed on your Ansible machine.
- Access to one or more hosts (like web servers) configured for management with Ansible.

---

## Duration

**Estimated Time:** 30 minutes

---

## Instructions

### Step 1: Create Your Playbook

1. On your Ansible machine, open a text editor and create a new file named `variable_play.yml`.

2. Add the following content to the file:

    ```yaml
    ---
    - hosts: webservers
      name: A play with variables
      gather_facts: no

      vars:
        first_name: John
        family: Doe

      tasks:
        - name: Print first name variable
          debug:
            var: first_name

        - name: Print family variable with message
          debug:
            msg: "Hello, {{ family }}"

        - name: Print both variables
          debug:
            msg: "{{ first_name }}, {{ family }}"
    ```

   **Explanation:**
    - `vars`: This section is used to define variables. Here, `first_name` and `family` are the two variables.
    - `tasks`: This section contains tasks that use the `debug` module to print variable values.

---

### Step 2: Run Your Playbook

1. Save the `variable_play.yml` file.

2. Execute the playbook using the following command:

    ```bash
    ansible-playbook variable_play.yml
    ```

3. Observe the output. You should see the values of `first_name` and `family` printed in different formats as specified in the tasks.

---

### Step 3: Experiment with Variables

- Try changing the values of `first_name` and `family` in the `vars` section and rerun the playbook to see how the output changes.
- Experiment with adding new variables and using them in tasks.

---

---

## Production Example: Multi-Environment Configuration Management

### Overview
This production example demonstrates how to manage configuration across multiple environments (dev, staging, prod) using variables and inventory groups.

### Step 1: Create Production Inventory Structure

1. Create an inventory file `production_inventory.yml`:

```yaml
all:
  children:
    webservers:
      hosts:
        web-prod-01:
          ansible_host: 10.0.1.10
        web-prod-02:
          ansible_host: 10.0.1.11
      vars:
        environment: production
        app_port: 8080
        db_host: prod-db.company.com
        ssl_enabled: true
        max_connections: 1000
    
    staging:
      hosts:
        web-staging-01:
          ansible_host: 10.0.2.10
      vars:
        environment: staging
        app_port: 8081
        db_host: staging-db.company.com
        ssl_enabled: false
        max_connections: 100
    
    development:
      hosts:
        web-dev-01:
          ansible_host: 10.0.3.10
      vars:
        environment: development
        app_port: 8082
        db_host: dev-db.company.com
        ssl_enabled: false
        max_connections: 10
```

### Step 2: Create Environment-Specific Variables

2. Create group variable files:

**group_vars/all.yml:**
```yaml
# Common variables for all environments
app_name: "MyWebApp"
app_version: "2.1.0"
log_level: "INFO"
backup_retention_days: 30

# Security settings
security_headers_enabled: true
csrf_protection: true
```

**group_vars/webservers.yml:**
```yaml
# Production-specific overrides
log_level: "WARN"
monitoring_enabled: true
performance_tuning: true
```

### Step 3: Create Production Playbook

3. Create `production_deploy.yml`:

```yaml
---
- name: Production Application Deployment
  hosts: "{{ target_env | default('webservers') }}"
  become: yes
  gather_facts: yes
  
  vars:
    # Runtime variables
    deployment_timestamp: "{{ ansible_date_time.epoch }}"
    deployment_user: "{{ ansible_user_id }}"
    
  pre_tasks:
    - name: Validate environment variables
      assert:
        that:
          - environment is defined
          - app_port is defined
          - db_host is defined
        fail_msg: "Required environment variables are missing"
    
    - name: Create deployment directory
      file:
        path: "/opt/{{ app_name }}/{{ environment }}"
        state: directory
        owner: "{{ app_name }}"
        group: "{{ app_name }}"
        mode: '0755'
  
  tasks:
    - name: Deploy application configuration
      template:
        src: app.conf.j2
        dest: "/opt/{{ app_name }}/{{ environment }}/app.conf"
        owner: "{{ app_name }}"
        group: "{{ app_name }}"
        mode: '0644'
      notify: restart application
    
    - name: Update application properties
      lineinfile:
        path: "/opt/{{ app_name }}/{{ environment }}/application.properties"
        regexp: "^{{ item.key }}="
        line: "{{ item.key }}={{ item.value }}"
        create: yes
      loop:
        - { key: "app.port", value: "{{ app_port }}" }
        - { key: "db.host", value: "{{ db_host }}" }
        - { key: "environment", value: "{{ environment }}" }
        - { key: "ssl.enabled", value: "{{ ssl_enabled }}" }
        - { key: "max.connections", value: "{{ max_connections }}" }
      notify: restart application
    
    - name: Log deployment information
      lineinfile:
        path: "/var/log/ansible-deployments.log"
        line: "{{ deployment_timestamp }} - {{ app_name }} {{ app_version }} deployed to {{ environment }} by {{ deployment_user }}"
        create: yes
  
  handlers:
    - name: restart application
      service:
        name: "{{ app_name }}"
        state: restarted
```

### Step 4: Create Application Configuration Template

4. Create `templates/app.conf.j2`:

```jinja2
# {{ app_name }} Configuration - {{ environment.upper() }}
# Generated on {{ ansible_date_time.iso8601 }}

[application]
name = {{ app_name }}
version = {{ app_version }}
environment = {{ environment }}
port = {{ app_port }}

[database]
host = {{ db_host }}
max_connections = {{ max_connections }}

[security]
ssl_enabled = {{ ssl_enabled | lower }}
{% if security_headers_enabled %}
security_headers = true
{% endif %}
{% if csrf_protection %}
csrf_protection = true
{% endif %}

[logging]
level = {{ log_level }}
{% if environment == 'production' %}
file = /var/log/{{ app_name }}/app.log
{% else %}
file = /tmp/{{ app_name }}.log
{% endif %}

[monitoring]
{% if monitoring_enabled is defined and monitoring_enabled %}
enabled = true
metrics_endpoint = /metrics
{% else %}
enabled = false
{% endif %}
```

### Step 5: Run Production Deployment

5. Deploy to different environments:

```bash
# Deploy to production
ansible-playbook production_deploy.yml -i production_inventory.yml

# Deploy to staging only
ansible-playbook production_deploy.yml -i production_inventory.yml -e "target_env=staging"

# Deploy specific version
ansible-playbook production_deploy.yml -i production_inventory.yml -e "app_version=2.1.1"
```

### Key Production Features:

- **Environment Isolation**: Separate configuration for each environment
- **Variable Validation**: Assertions to ensure required variables are present
- **Configuration Templates**: Dynamic configuration generation
- **Audit Logging**: Deployment tracking and logging
- **Error Handling**: Proper validation and error messages
- **Scalability**: Inventory structure supports multiple servers per environment

---

## Conclusion

Well done! You've completed a basic lab on using variables in Ansible playbooks. Understanding variables is crucial as they allow you to write more dynamic and reusable playbooks. The production example demonstrates enterprise-grade variable management across multiple environments. Keep experimenting with different types of variables and their applications in your Ansible projects. 👏

