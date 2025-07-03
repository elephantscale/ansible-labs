# Lab: Creating Your First Ansible Playbook

## Overview

In this lab, you will create and run your first Ansible playbook. A playbook is a YAML file where you define the tasks and settings for Ansible to execute on your hosts. 

You'll learn the basics of playbook structure and how to perform simple tasks.

---

## Objectives

- Understand the structure of an Ansible playbook.
- Create a playbook with basic tasks.
- Run the playbook on a group of web servers.

---

## Prerequisites

- Basic knowledge of YAML syntax.
- Ansible installed on your Ansible machine (the machine where you run Ansible commands).
- Access to one or more hosts (like web servers) where you have SSH access and Ansible is configured to manage them.

---

## Duration

**Estimated Time:** 30 minutes

---

## Instructions

### Step 1: Create Your Playbook File

1. On your Ansible machine, open a text editor and create a new file named `first_play.yml`.

2. Add the following content to the file:

    ```yaml
    ---
    - name: Our First Play
      hosts: webservers
      gather_facts: no

      tasks:
        - name: Print something
          debug:
            msg: "Hello World"

        - name: Print something else
          debug:
            msg: "Good bye"
    ```

   Here's a breakdown of what each part does:
    - `---` signifies the start of a YAML file.
    - The first section defines the play:
        - `name`: A human-readable name for the play.
        - `hosts`: Specifies the group of hosts to target, in this case, `webservers`.
        - `gather_facts`: Set to `no` to skip gathering system facts (to speed up execution for this simple task).
    - Under `tasks`, we define what we want Ansible to do:
        - Each `- name` is a human-readable description of the task.
        - `debug` is a module provided by Ansible to print messages to the console.

---

### Step 2: Run Your Playbook

1. Save the `first_play.yml` file.

2. Run the playbook using the `ansible-playbook` command:

    ```bash
    ansible-playbook first_play.yml
    ```

3. Observe the output. Ansible will connect to the hosts in the `webservers` group and execute the tasks you defined. You should see "Hello World" and "Good bye" messages in the output.

---

### Step 3: Enhanced Examples - From Simple to Production

#### Simple Example (What you just did)
The basic debug messages you created above.

#### Intermediate Example - Adding System Information
Try this enhanced version:

```yaml
---
- name: Enhanced First Play
  hosts: webservers
  gather_facts: yes
  
  tasks:
    - name: Display welcome message
      debug:
        msg: "Welcome to {{ inventory_hostname }}!"
    
    - name: Show system information
      debug:
        msg: "Running {{ ansible_facts['distribution'] }} {{ ansible_facts['distribution_version'] }}"
    
    - name: Display current time
      debug:
        var: ansible_date_time.iso8601
```

#### Production-Ready Example - Web Server Health Check
For production environments, you might use:

```yaml
---
- name: Web Server Health Check
  hosts: webservers
  gather_facts: yes
  
  tasks:
    - name: Check if web service is running
      service_facts:
    
    - name: Display web server status
      debug:
        msg: "Apache service is {{ ansible_facts.services['apache2.service'].state | default('not found') }}"
      when: "'apache2.service' in ansible_facts.services"
    
    - name: Check disk usage
      debug:
        msg: "Disk usage on /: {{ (ansible_facts['mounts'] | selectattr('mount', 'equalto', '/') | list | first)['size_available'] // 1024 // 1024 // 1024 }} GB available"
    
    - name: Log health check completion
      debug:
        msg: "Health check completed at {{ ansible_date_time.iso8601 }} for {{ inventory_hostname }}"
```

### Step 4: Try the Examples

1. Save each example as separate files (`enhanced_play.yml`, `health_check.yml`)
2. Run them and observe the differences in output complexity
3. Notice how we progress from simple debug messages to actual system monitoring

---

## Conclusion

Congratulations! You've successfully created and run your first Ansible playbook and seen how it can scale from simple tasks to production-ready automation. 

**What you learned:**
- Basic playbook structure
- How to progress from simple to complex automation
- The power of Ansible facts for system information
- Production patterns for monitoring and health checks

This foundational skill is a stepping stone to more complex Ansible automation tasks. As you become more comfortable with playbooks, you'll be able to automate a wide range of tasks on your managed hosts. 👏

