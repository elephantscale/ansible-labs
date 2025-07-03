# Lab Instructions: Using Conditionals in Ansible Playbooks

## Overview

In this lab, you will learn how to use conditionals in an Ansible playbook. Specifically, you will understand how the `when` clause can be used to control the execution of tasks based on the value of a variable.

---

## Objectives

- Learn how to use the `when` clause to implement conditionals.
- Create a task that executes only under specific conditions.

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

1. Create a new YAML file named `conditional_playbook.yml`.

2. Define the play's name, target hosts (`all` in this case), and whether to gather facts:

   ```yaml
   ---
   - name: A conditional play
     hosts: all
     gather_facts: no
   ```

   **Explanation:**
   - `hosts: all` targets all hosts in the inventory.
   - `gather_facts: no` skips automatic fact gathering to keep the playbook simple.

---

### Step 2: Define a Variable

1. In the `vars` section, define a variable named `epic` and assign it a boolean value:

   ```yaml
     vars:
       epic: true
   ```

   **Explanation:**
   - The variable `epic` will be used to control the execution of tasks.

---

### Step 3: Add a Conditional Task

1. Under the `tasks` section, add a task that prints a message "Hello A" if the variable `epic` is `true`:

   ```yaml
     tasks:
       - name: Task A
         debug:
           msg: "Hello A"
         when: epic
   ```

   **Explanation:**
   - The `when` clause ensures the task runs only when the condition (`epic: true`) is met.

---

### Step 4: Execute the Playbook

1. Save the `conditional_playbook.yml` file.

2. Run the playbook using the following command:

   ```bash
   ansible-playbook conditional_playbook.yml
   ```

3. Observe the output. The task "Task A" will execute only if `epic` is set to `true`.

---

### Step 5: Progressive Examples - Simple to Production

#### Intermediate Example - OS-Specific Tasks
Create `os_specific.yml` with realistic conditionals:

```yaml
---
- name: OS-Specific Package Management
  hosts: all
  gather_facts: yes
  
  tasks:
    - name: Install package on Ubuntu/Debian
      debug:
        msg: "Would install using apt: {{ package_name | default('nginx') }}"
      when: ansible_facts['os_family'] == "Debian"
    
    - name: Install package on RedHat/CentOS
      debug:
        msg: "Would install using yum/dnf: {{ package_name | default('nginx') }}"
      when: ansible_facts['os_family'] == "RedHat"
    
    - name: Skip unsupported OS
      debug:
        msg: "OS {{ ansible_facts['os_family'] }} not supported"
      when: ansible_facts['os_family'] not in ["Debian", "RedHat"]
```

#### Production Example - Environment-Based Configuration
Create `production_conditional.yml`:

```yaml
---
- name: Environment-Based Web Server Configuration
  hosts: webservers
  gather_facts: yes
  vars:
    environment: "{{ env | default('development') }}"
    max_connections:
      development: 50
      staging: 100
      production: 500
  
  tasks:
    - name: Set development configuration
      debug:
        msg: "Configuring for development with {{ max_connections.development }} max connections"
      when: environment == "development"
    
    - name: Set production configuration
      debug:
        msg: "Configuring for production with {{ max_connections.production }} max connections"
      when: environment == "production"
    
    - name: Enable debug logging in non-production
      debug:
        msg: "Debug logging enabled for {{ environment }} environment"
      when: environment != "production"
    
    - name: Security hardening for production
      debug:
        msg: "Applying production security hardening"
      when: 
        - environment == "production"
        - ansible_facts['processor_count'] >= 2
    
    - name: Warning for low memory in production
      debug:
        msg: "WARNING: Production server has low memory ({{ ansible_facts['memtotal_mb'] }}MB)"
      when: 
        - environment == "production"
        - ansible_facts['memtotal_mb'] < 4096
```

#### Advanced Example - Complex Business Logic
Create `business_logic.yml`:

```yaml
---
- name: Business Logic Conditionals
  hosts: all
  gather_facts: yes
  vars:
    business_hours: "{{ ansible_date_time.hour | int >= 9 and ansible_date_time.hour | int <= 17 }}"
    is_weekend: "{{ ansible_date_time.weekday | int in [5, 6] }}"
    maintenance_window: "{{ ansible_date_time.hour | int >= 2 and ansible_date_time.hour | int <= 4 }}"
  
  tasks:
    - name: Schedule updates during maintenance window
      debug:
        msg: "Updates can be performed now (maintenance window)"
      when: maintenance_window and is_weekend
    
    - name: Defer updates during business hours
      debug:
        msg: "Deferring updates until maintenance window (currently business hours)"
      when: business_hours and not is_weekend
    
    - name: Check system resources before deployment
      debug:
        msg: "System ready for deployment"
      when:
        - ansible_facts['memfree_mb'] > 1024
        - (ansible_facts['mounts'] | selectattr('mount', 'equalto', '/') | list | first)['size_available'] > 5000000000
        - ansible_facts['processor_count'] >= 2
```

### Step 6: Run the Enhanced Examples

1. Test the OS-specific example:
   ```bash
   ansible-playbook os_specific.yml
   ```

2. Test environment-based configuration:
   ```bash
   ansible-playbook production_conditional.yml -e "env=production"
   ansible-playbook production_conditional.yml -e "env=development"
   ```

3. Test business logic:
   ```bash
   ansible-playbook business_logic.yml
   ```

---

## Conclusion

In this lab, you've learned how to use conditionals in Ansible playbooks, progressing from simple boolean checks to complex production-ready logic.

**What you learned:**
- Basic `when` clause usage
- OS-specific conditional logic
- Environment-based configurations
- Complex multi-condition logic
- Real-world business logic implementation

**Key Conditional Patterns:**
- Single condition: `when: variable`
- Multiple conditions: `when: condition1 and condition2`
- List conditions: `when: variable in ["value1", "value2"]`
- Complex logic: `when: (condition1 or condition2) and condition3`

These patterns form the foundation for building sophisticated, environment-aware automation! 👏

