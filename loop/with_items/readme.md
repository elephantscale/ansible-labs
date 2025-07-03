# Lab: Iterating Over Items with Ansible

## Overview

In this lab, you will learn how to use the `with_items` directive in Ansible to iterate over a list of items and execute tasks for each item. By the end, you'll be familiar with iterating over a list in a playbook and displaying a customized message for each list item.

---

## Objectives

- Use the `with_items` directive to iterate over lists.
- Understand alternative iteration methods including `loop`.
- Execute tasks dynamically for each item in a list.

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

1. Create a new YAML file named `item_iteration.yml`.

2. Define the play's name, target hosts, and enable privilege escalation:

   ```yaml
   ---
   - name: Iterate over a list of items
     hosts: webservers
     become: yes
   ```

---

### Step 2: Add the Iteration Task

1. Under the `tasks` section, add a task to iterate over a list of names and print a custom message for each:

   ```yaml
     tasks:
       - name: Print a message for each item
         debug:
           msg: "Hello, {{ item }}"
         with_items:
           - John
           - Jane
           - Alex
           - Sarah
   ```

   **Explanation:**
   - `with_items`: Iterates over the provided list.
   - `{{ item }}`: Refers to the current item in the iteration.

---

### Step 3: Execute the Playbook

1. Save the `item_iteration.yml` file.

2. Run the playbook using the following command:

   ```bash
   ansible-playbook item_iteration.yml
   ```

3. Observe the output. The playbook will print a custom message for each name in the list.

---

## Solution File

You can compare your playbook with the [item_iteration.yml](item_iteration.yml) file in the current directory.

---

## Conclusion

In this lab, you learned how to use the `with_items` directive in Ansible to iterate over lists and execute tasks multiple times with different data. This technique is valuable for automating repetitive operations and making playbooks more dynamic and efficient.

### Alternative: Modern Loop Syntax

While `with_items` works perfectly, you can also use the newer `loop` syntax:

```yaml
- name: Print a message for each item (modern syntax)
  debug:
    msg: "Hello, {{ item }}"
  loop:
    - John
    - Jane
    - Alex
    - Sarah
```

### Key Takeaways:
- `with_items` is a reliable and widely-used approach
- `loop` is an alternative modern syntax
- Both approaches produce the same results
- Choose the syntax your team prefers

---

## Production Example: Multi-Server Application Deployment

### Overview
This production example demonstrates how to use loops for deploying a multi-tier application across multiple servers with complex configuration management, service orchestration, and infrastructure provisioning.

### Step 1: Create Production Inventory

1. Create an inventory file `production_deployment_inventory.yml`:

```yaml
all:
  children:
    loadbalancers:
      hosts:
        lb-prod-01:
          ansible_host: 10.0.1.10
          nginx_upstream_servers:
            - { name: "web-prod-01", ip: "10.0.2.10", port: 8080, weight: 3 }
            - { name: "web-prod-02", ip: "10.0.2.11", port: 8080, weight: 3 }
            - { name: "web-prod-03", ip: "10.0.2.12", port: 8080, weight: 2 }
        lb-prod-02:
          ansible_host: 10.0.1.11
          nginx_upstream_servers:
            - { name: "web-prod-01", ip: "10.0.2.10", port: 8080, weight: 3 }
            - { name: "web-prod-02", ip: "10.0.2.11", port: 8080, weight: 3 }
            - { name: "web-prod-03", ip: "10.0.2.12", port: 8080, weight: 2 }
    
    webservers:
      hosts:
        web-prod-01:
          ansible_host: 10.0.2.10
          app_instances:
            - { name: "app-primary", port: 8080, memory: "2048m", profile: "production" }
            - { name: "app-worker", port: 8081, memory: "1024m", profile: "worker" }
        web-prod-02:
          ansible_host: 10.0.2.11
          app_instances:
            - { name: "app-primary", port: 8080, memory: "2048m", profile: "production" }
            - { name: "app-worker", port: 8081, memory: "1024m", profile: "worker" }
        web-prod-03:
          ansible_host: 10.0.2.12
          app_instances:
            - { name: "app-primary", port: 8080, memory: "1536m", profile: "production" }
    
    databases:
      hosts:
        db-prod-01:
          ansible_host: 10.0.3.10
          database_role: primary
          databases:
            - { name: "webapp_prod", owner: "webapp", encoding: "UTF8", collate: "en_US.UTF-8" }
            - { name: "analytics_prod", owner: "analytics", encoding: "UTF8", collate: "en_US.UTF-8" }
            - { name: "sessions_prod", owner: "webapp", encoding: "UTF8", collate: "en_US.UTF-8" }
        db-prod-02:
          ansible_host: 10.0.3.11
          database_role: replica
          databases:
            - { name: "webapp_prod", owner: "webapp", encoding: "UTF8", collate: "en_US.UTF-8" }
            - { name: "analytics_prod", owner: "analytics", encoding: "UTF8", collate: "en_US.UTF-8" }
            - { name: "sessions_prod", owner: "webapp", encoding: "UTF8", collate: "en_US.UTF-8" }
    
    monitoring:
      hosts:
        monitor-prod-01:
          ansible_host: 10.0.4.10
          monitoring_services:
            - { name: "prometheus", port: 9090, config: "prometheus.yml" }
            - { name: "grafana", port: 3000, config: "grafana.ini" }
            - { name: "alertmanager", port: 9093, config: "alertmanager.yml" }
```

### Step 2: Create Production Deployment Playbook

2. Create `production_deployment.yml`:

```yaml
---
- name: Production Multi-Server Application Deployment
  hosts: all
  become: yes
  gather_facts: yes
  
  vars:
    # Application configuration
    app_name: "webapp"
    app_version: "2.1.0"
    deployment_environments:
      - { name: "production", config_suffix: "prod", log_level: "WARN" }
      - { name: "staging", config_suffix: "stage", log_level: "INFO" }
      - { name: "development", config_suffix: "dev", log_level: "DEBUG" }
    
    # Infrastructure components
    infrastructure_packages:
      - { name: "nginx", service: "nginx", config_path: "/etc/nginx" }
      - { name: "postgresql", service: "postgresql", config_path: "/etc/postgresql/14/main" }
      - { name: "redis-server", service: "redis", config_path: "/etc/redis" }
      - { name: "openjdk-11-jdk", service: null, config_path: null }
    
    # Security and monitoring
    security_tools:
      - { name: "fail2ban", service: "fail2ban", config: "/etc/fail2ban/jail.local" }
      - { name: "ufw", service: "ufw", config: "/etc/ufw/ufw.conf" }
      - { name: "auditd", service: "auditd", config: "/etc/audit/auditd.conf" }
    
    # SSL certificates for different domains
    ssl_certificates:
      - { domain: "webapp.company.com", cert_path: "/etc/ssl/certs/webapp.crt", key_path: "/etc/ssl/private/webapp.key" }
      - { domain: "api.company.com", cert_path: "/etc/ssl/certs/api.crt", key_path: "/etc/ssl/private/api.key" }
      - { domain: "admin.company.com", cert_path: "/etc/ssl/certs/admin.crt", key_path: "/etc/ssl/private/admin.key" }
  
  tasks:
    # Install infrastructure packages across all servers
    - name: Install infrastructure packages
      package:
        name: "{{ item.name }}"
        state: present
        update_cache: yes
      with_items: "{{ infrastructure_packages }}"
      register: package_installation
      
    - name: Enable and start infrastructure services
      service:
        name: "{{ item.service }}"
        state: started
        enabled: yes
      with_items: "{{ infrastructure_packages }}"
      when: item.service is not none
      
    # Security hardening across all servers
    - name: Install security tools
      package:
        name: "{{ item.name }}"
        state: present
      with_items: "{{ security_tools }}"
      
    - name: Configure security tools
      template:
        src: "{{ item.name }}.conf.j2"
        dest: "{{ item.config }}"
        backup: yes
        mode: '0644'
      with_items: "{{ security_tools }}"
      notify: restart security services
      
    # Create application users and directories
    - name: Create application users
      user:
        name: "{{ item }}"
        home: "/opt/{{ item }}"
        shell: /bin/bash
        create_home: yes
        system: yes
      with_items:
        - "{{ app_name }}"
        - "{{ app_name }}-worker"
        - "{{ app_name }}-admin"
      
    - name: Create application directories
      file:
        path: "{{ item }}"
        state: directory
        owner: "{{ app_name }}"
        group: "{{ app_name }}"
        mode: '0755'
      with_items:
        - "/opt/{{ app_name }}/config"
        - "/opt/{{ app_name }}/logs"
        - "/opt/{{ app_name }}/data"
        - "/opt/{{ app_name }}/backups"
        - "/opt/{{ app_name }}/temp"

# Load Balancer Configuration
- name: Configure Load Balancers
  hosts: loadbalancers
  become: yes
  
  tasks:
    - name: Create nginx upstream configuration
      template:
        src: nginx-upstream.conf.j2
        dest: "/etc/nginx/conf.d/{{ item.name }}-upstream.conf"
        backup: yes
      with_items: "{{ nginx_upstream_servers }}"
      notify: reload nginx
      
    - name: Configure SSL certificates
      copy:
        src: "{{ item.cert_path }}"
        dest: "{{ item.cert_path }}"
        owner: root
        group: root
        mode: '0644'
      with_items: "{{ ssl_certificates }}"
      notify: reload nginx
      
    - name: Configure SSL private keys
      copy:
        src: "{{ item.key_path }}"
        dest: "{{ item.key_path }}"
        owner: root
        group: root
        mode: '0600'
      with_items: "{{ ssl_certificates }}"
      notify: reload nginx
      
    - name: Create virtual host configurations
      template:
        src: nginx-vhost.conf.j2
        dest: "/etc/nginx/sites-available/{{ item.domain }}"
        backup: yes
      with_items: "{{ ssl_certificates }}"
      notify: reload nginx
      
    - name: Enable virtual hosts
      file:
        src: "/etc/nginx/sites-available/{{ item.domain }}"
        dest: "/etc/nginx/sites-enabled/{{ item.domain }}"
        state: link
      with_items: "{{ ssl_certificates }}"
      notify: reload nginx

# Web Server Configuration
- name: Configure Web Servers
  hosts: webservers
  become: yes
  
  tasks:
    - name: Deploy application instances
      template:
        src: application.conf.j2
        dest: "/opt/{{ app_name }}/config/{{ item.name }}.conf"
        owner: "{{ app_name }}"
        group: "{{ app_name }}"
        mode: '0644'
      with_items: "{{ app_instances }}"
      notify: restart application instances
      
    - name: Create systemd service files for application instances
      template:
        src: webapp-instance.service.j2
        dest: "/etc/systemd/system/{{ app_name }}-{{ item.name }}.service"
        mode: '0644'
      with_items: "{{ app_instances }}"
      notify: reload systemd
      
    - name: Enable and start application instances
      service:
        name: "{{ app_name }}-{{ item.name }}"
        state: started
        enabled: yes
        daemon_reload: yes
      with_items: "{{ app_instances }}"
      
    - name: Configure application logging
      template:
        src: logrotate.conf.j2
        dest: "/etc/logrotate.d/{{ app_name }}-{{ item.name }}"
        mode: '0644'
      with_items: "{{ app_instances }}"
      
    - name: Create health check endpoints
      template:
        src: health-check.sh.j2
        dest: "/opt/{{ app_name }}/bin/health-check-{{ item.name }}.sh"
        owner: "{{ app_name }}"
        group: "{{ app_name }}"
        mode: '0755'
      with_items: "{{ app_instances }}"

# Database Configuration
- name: Configure Database Servers
  hosts: databases
  become: yes
  
  tasks:
    - name: Create databases
      postgresql_db:
        name: "{{ item.name }}"
        owner: "{{ item.owner }}"
        encoding: "{{ item.encoding }}"
        lc_collate: "{{ item.collate }}"
        lc_ctype: "{{ item.collate }}"
        state: present
      with_items: "{{ databases }}"
      when: database_role == "primary"
      
    - name: Create database users
      postgresql_user:
        name: "{{ item.owner }}"
        password: "{{ vault_db_passwords[item.owner] }}"
        db: "{{ item.name }}"
        priv: "ALL"
        state: present
      with_items: "{{ databases }}"
      when: database_role == "primary"
      
    - name: Configure database backup scripts
      template:
        src: db-backup.sh.j2
        dest: "/opt/backup/{{ item.name }}-backup.sh"
        owner: postgres
        group: postgres
        mode: '0755'
      with_items: "{{ databases }}"
      
    - name: Schedule database backups
      cron:
        name: "Backup {{ item.name }}"
        minute: "{{ (item.name | hash | int) % 60 }}"
        hour: "2"
        job: "/opt/backup/{{ item.name }}-backup.sh"
        user: postgres
      with_items: "{{ databases }}"

# Monitoring Configuration
- name: Configure Monitoring Servers
  hosts: monitoring
  become: yes
  
  tasks:
    - name: Deploy monitoring service configurations
      template:
        src: "{{ item.config }}.j2"
        dest: "/etc/{{ item.name }}/{{ item.config }}"
        owner: "{{ item.name }}"
        group: "{{ item.name }}"
        mode: '0644'
        backup: yes
      with_items: "{{ monitoring_services }}"
      notify: restart monitoring services
      
    - name: Create monitoring service directories
      file:
        path: "{{ item }}"
        state: directory
        owner: prometheus
        group: prometheus
        mode: '0755'
      with_items:
        - "/var/lib/prometheus"
        - "/etc/prometheus/rules"
        - "/etc/prometheus/targets"
      
    - name: Deploy monitoring rules
      template:
        src: "{{ item }}.rules.j2"
        dest: "/etc/prometheus/rules/{{ item }}.rules"
        owner: prometheus
        group: prometheus
        mode: '0644'
      with_items:
        - "webapp-alerts"
        - "infrastructure-alerts"
        - "database-alerts"
      notify: reload prometheus
      
    - name: Configure monitoring targets
      template:
        src: targets.json.j2
        dest: "/etc/prometheus/targets/{{ item }}.json"
        owner: prometheus
        group: prometheus
        mode: '0644'
      with_items:
        - "webservers"
        - "databases"
        - "loadbalancers"
      notify: reload prometheus

  handlers:
    - name: restart security services
      service:
        name: "{{ item.service }}"
        state: restarted
      with_items: "{{ security_tools }}"
      
    - name: reload nginx
      service:
        name: nginx
        state: reloaded
        
    - name: restart application instances
      service:
        name: "{{ app_name }}-{{ item.name }}"
        state: restarted
      with_items: "{{ app_instances }}"
      
    - name: reload systemd
      systemd:
        daemon_reload: yes
        
    - name: restart monitoring services
      service:
        name: "{{ item.name }}"
        state: restarted
      with_items: "{{ monitoring_services }}"
      
    - name: reload prometheus
      service:
        name: prometheus
        state: reloaded
```

### Step 3: Create Complex Loop Example

3. Create `complex_loops.yml`:

```yaml
---
- name: Complex Loop Operations
  hosts: webservers
  become: yes
  
  vars:
    # Complex data structure for application deployment
    applications:
      - name: "frontend"
        port: 3000
        instances: 3
        environments:
          - { name: "prod", replicas: 3, memory: "512m" }
          - { name: "stage", replicas: 2, memory: "256m" }
        dependencies:
          - { name: "nodejs", version: "18.x" }
          - { name: "npm", version: "latest" }
          - { name: "pm2", version: "latest" }
      
      - name: "backend"
        port: 8080
        instances: 2
        environments:
          - { name: "prod", replicas: 2, memory: "1024m" }
          - { name: "stage", replicas: 1, memory: "512m" }
        dependencies:
          - { name: "openjdk-11-jdk", version: "latest" }
          - { name: "maven", version: "3.8.x" }
  
  tasks:
    # Nested loops for complex deployment
    - name: Install application dependencies
      package:
        name: "{{ item.1.name }}"
        state: present
      with_subelements:
        - "{{ applications }}"
        - dependencies
      register: dependency_installation
      
    - name: Create application directories
      file:
        path: "/opt/{{ item.0.name }}/{{ item.1.name }}"
        state: directory
        owner: "{{ item.0.name }}"
        group: "{{ item.0.name }}"
        mode: '0755'
      with_nested:
        - "{{ applications }}"
        - "{{ applications[0].environments }}"
      
    - name: Deploy application configurations
      template:
        src: "{{ item.0.name }}.conf.j2"
        dest: "/opt/{{ item.0.name }}/{{ item.1.name }}/config.conf"
        owner: "{{ item.0.name }}"
        group: "{{ item.0.name }}"
        mode: '0644'
      with_nested:
        - "{{ applications }}"
        - "{{ applications[0].environments }}"
      
    # Loop with conditional logic
    - name: Start application instances
      service:
        name: "{{ item.0.name }}-{{ item.1.name }}"
        state: started
        enabled: yes
      with_nested:
        - "{{ applications }}"
        - "{{ applications[0].environments }}"
      when: item.1.name == "prod"
      
    # Loop with complex filtering
    - name: Configure production monitoring
      template:
        src: monitoring.conf.j2
        dest: "/etc/monitoring/{{ item.name }}-prod.conf"
        mode: '0644'
      with_items: "{{ applications }}"
      when: item.port | int > 5000
      
    # Loop with sequential execution
    - name: Database migration tasks
      command: >
        /opt/{{ item.0.name }}/bin/migrate.sh
        --environment {{ item.1.name }}
        --replicas {{ item.1.replicas }}
      with_nested:
        - "{{ applications }}"
        - "{{ applications[0].environments }}"
      when: item.0.name == "backend"
      run_once: true
      delegate_to: "{{ groups['webservers'][0] }}"
```

### Step 4: Create Multi-Environment Loop Example

4. Create `multi_environment_loops.yml`:

```yaml
---
- name: Multi-Environment Configuration Management
  hosts: all
  become: yes
  
  vars:
    # Environment-specific configurations
    environments:
      - name: "production"
        config:
          db_pool_size: 50
          cache_size: "1024m"
          log_level: "WARN"
          backup_retention: 90
        features:
          - { name: "ssl", enabled: true }
          - { name: "monitoring", enabled: true }
          - { name: "debug", enabled: false }
      
      - name: "staging"
        config:
          db_pool_size: 20
          cache_size: "512m"
          log_level: "INFO"
          backup_retention: 30
        features:
          - { name: "ssl", enabled: true }
          - { name: "monitoring", enabled: true }
          - { name: "debug", enabled: true }
      
      - name: "development"
        config:
          db_pool_size: 5
          cache_size: "256m"
          log_level: "DEBUG"
          backup_retention: 7
        features:
          - { name: "ssl", enabled: false }
          - { name: "monitoring", enabled: false }
          - { name: "debug", enabled: true }
  
  tasks:
    - name: Deploy environment-specific configurations
      template:
        src: app-config.j2
        dest: "/opt/{{ app_name }}/config/{{ item.name }}.conf"
        owner: "{{ app_name }}"
        group: "{{ app_name }}"
        mode: '0644'
        backup: yes
      with_items: "{{ environments }}"
      
    - name: Configure environment-specific features
      lineinfile:
        path: "/opt/{{ app_name }}/config/{{ item.0.name }}.conf"
        regexp: "^{{ item.1.name }}_enabled="
        line: "{{ item.1.name }}_enabled={{ item.1.enabled | lower }}"
        create: yes
      with_subelements:
        - "{{ environments }}"
        - features
      notify: restart application
```

### Step 5: Run Production Deployment

5. Deploy the complex multi-server setup:

```bash
# Full production deployment
ansible-playbook production_deployment.yml -i production_deployment_inventory.yml

# Deploy to specific server groups
ansible-playbook production_deployment.yml -i production_deployment_inventory.yml -l webservers

# Deploy with complex loops
ansible-playbook complex_loops.yml -i production_deployment_inventory.yml -l webservers

# Deploy multi-environment configurations
ansible-playbook multi_environment_loops.yml -i production_deployment_inventory.yml -e "app_name=webapp"
```

### Key Production Features:

- **Complex Data Structures**: Nested loops with subelements and multi-dimensional data
- **Conditional Loop Execution**: Loops with when clauses for selective deployment
- **Service Orchestration**: Coordinated deployment across multiple server types
- **Configuration Management**: Environment-specific settings using loops
- **Infrastructure as Code**: Complete infrastructure provisioning with loops
- **Error Handling**: Proper error handling and rollback strategies
- **Scalability**: Easily add new servers or environments through inventory

---

Practice applying list iteration to various tasks in your Ansible playbooks to enhance your automation capabilities! 👏

