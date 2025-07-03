# Installing Apache2 with Handlers in Ansible

## Duration

Approximately 30 minutes

## Objective

In this lab, you will learn how to use Ansible handlers to respond to change events in your tasks. Specifically, you'll be installing the `apache2` package and, upon its successful installation, triggering handlers to display success messages.

## Prerequisites:

- Ansible installed on your machine.
- A target host or set of hosts configured in your Ansible inventory under the group `webservers`.
- The target host should have `apt` package manager (typically an Ubuntu/Debian machine).

## Step-by-step guide:

### Step A: Setting up the Playbook

- Start by creating a new playbook named `apache_install_handlers.yml`.
- Define the playbook structure:

```yaml
---
- hosts: webservers
  name: Install Apache2 with Handlers
  become: yes
  become_user: root
  gather_facts: no
```

### Step B: Installing Apache2 Package

- Add the task that installs the Apache2 package using the `apt` module:

```yaml
  tasks:
    - name: update package
      apt: 
        update_cache: yes
      
    - name: Install Package
      apt:
        name: apache2
      notify:
        - sucs_msg
        - sucs_msg_b
```

This task instructs Ansible to install the `apache2` package. If the package installation changes the state of the machine (e.g., installing or updating the package), the `notify` directive will trigger the named handlers.

### Step C: Define Handlers for Post-installation Messages

- Beneath the tasks, define handlers that display success messages:

```yaml
  handlers:
    - name: sucs_msg
      debug:
        msg: Package has been Installed

    - name: sucs_msg_b
      debug:
        msg: Package has been Installed B
```

Handlers are special types of tasks in Ansible that run only if notified by another task. In our playbook, upon successful installation of the Apache2 package, both handlers will be triggered, displaying their respective success messages.

### Step D: Running the Playbook

- Save the `apache_install_handlers.yml` file.
- Execute the playbook using the following command:

```bash
ansible-playbook apache_install_handlers.yml
```

Watch as Ansible installs the Apache2 package on the target `webservers` host(s). If the package gets installed or updated, you'll see the success messages from the handlers.

## Final File

You can compare your playbook with the [apache_install_handlers.yml](apache_install_handlers.yml) file in the current directory.

---

## Production Example: Enterprise Web Application Deployment with Orchestrated Service Management

### Overview
This production example demonstrates advanced handler usage for orchestrating complex service dependencies, configuration management, and rolling deployments in an enterprise environment.

### Step 1: Create Production Infrastructure Inventory

1. Create an inventory file `enterprise_deployment_inventory.yml`:

```yaml
all:
  children:
    loadbalancers:
      hosts:
        lb-prod-01:
          ansible_host: 10.0.1.10
          haproxy_role: primary
        lb-prod-02:
          ansible_host: 10.0.1.11
          haproxy_role: backup
      vars:
        lb_algorithm: "roundrobin"
        health_check_interval: 5000
        backend_servers:
          - { name: "web-01", ip: "10.0.2.10", port: 80, weight: 100 }
          - { name: "web-02", ip: "10.0.2.11", port: 80, weight: 100 }
          - { name: "web-03", ip: "10.0.2.12", port: 80, weight: 75 }
    
    webservers:
      hosts:
        web-prod-01:
          ansible_host: 10.0.2.10
          server_role: primary
        web-prod-02:
          ansible_host: 10.0.2.11
          server_role: secondary
        web-prod-03:
          ansible_host: 10.0.2.12
          server_role: secondary
      vars:
        app_port: 8080
        ssl_port: 8443
        monitoring_port: 9090
    
    databases:
      hosts:
        db-prod-01:
          ansible_host: 10.0.3.10
          db_role: master
        db-prod-02:
          ansible_host: 10.0.3.11
          db_role: slave
      vars:
        mysql_port: 3306
        replication_user: "replicator"
    
    cache_servers:
      hosts:
        cache-prod-01:
          ansible_host: 10.0.4.10
          redis_role: master
        cache-prod-02:
          ansible_host: 10.0.4.11
          redis_role: slave
      vars:
        redis_port: 6379
        redis_max_memory: "2gb"
```

### Step 2: Create Enterprise Deployment Playbook

2. Create `enterprise_deployment.yml`:

```yaml
---
- name: Enterprise Web Application Deployment
  hosts: all
  become: yes
  gather_facts: yes
  serial: "{{ deployment_batch_size | default(1) }}"
  
  vars:
    app_name: "enterprise-webapp"
    app_version: "3.2.1"
    deployment_timestamp: "{{ ansible_date_time.epoch }}"
    config_checksum: "{{ ansible_date_time.iso8601 | hash('sha256') }}"
    
    # Service dependency order
    service_start_order:
      - mysql
      - redis
      - nginx
      - "{{ app_name }}"
      - haproxy
      
    # Configuration files that trigger service restarts
    critical_configs:
      - "/etc/nginx/nginx.conf"
      - "/etc/nginx/sites-enabled/{{ app_name }}"
      - "/etc/mysql/mysql.conf.d/mysqld.cnf"
      - "/etc/redis/redis.conf"
      - "/etc/haproxy/haproxy.cfg"
      - "/opt/{{ app_name }}/config/application.properties"
  
  pre_tasks:
    - name: Create deployment backup directory
      file:
        path: "/opt/backups/{{ deployment_timestamp }}"
        state: directory
        mode: '0755'
    
    - name: Backup current configuration files
      copy:
        src: "{{ item }}"
        dest: "/opt/backups/{{ deployment_timestamp }}/{{ item | basename }}"
        remote_src: yes
      with_items: "{{ critical_configs }}"
      ignore_errors: yes
      
    - name: Stop application for maintenance
      service:
        name: "{{ app_name }}"
        state: stopped
      when: maintenance_mode | default(false) | bool
      
  tasks:
    # Database Configuration
    - name: Configure MySQL master server
      template:
        src: mysql-master.cnf.j2
        dest: /etc/mysql/mysql.conf.d/mysqld.cnf
        owner: mysql
        group: mysql
        mode: '0644'
        backup: yes
      when: db_role == 'master'
      notify:
        - restart mysql
        - wait for mysql
        - configure replication user
        - restart dependent services
    
    - name: Configure MySQL slave server
      template:
        src: mysql-slave.cnf.j2
        dest: /etc/mysql/mysql.conf.d/mysqld.cnf
        owner: mysql
        group: mysql
        mode: '0644'
        backup: yes
      when: db_role == 'slave'
      notify:
        - restart mysql
        - wait for mysql
        - configure slave replication
        - restart dependent services
    
    # Cache Configuration
    - name: Configure Redis master
      template:
        src: redis-master.conf.j2
        dest: /etc/redis/redis.conf
        owner: redis
        group: redis
        mode: '0644'
        backup: yes
      when: redis_role == 'master'
      notify:
        - restart redis master
        - wait for redis master
        - notify redis slaves
        - validate cache cluster
    
    - name: Configure Redis slave
      template:
        src: redis-slave.conf.j2
        dest: /etc/redis/redis.conf
        owner: redis
        group: redis
        mode: '0644'
        backup: yes
      when: redis_role == 'slave'
      notify:
        - restart redis slave
        - wait for redis slave
        - validate slave sync
    
    # Web Server Configuration
    - name: Deploy Nginx main configuration
      template:
        src: nginx.conf.j2
        dest: /etc/nginx/nginx.conf
        owner: root
        group: root
        mode: '0644'
        backup: yes
      notify:
        - validate nginx config
        - reload nginx
        - notify load balancers
    
    - name: Deploy application virtual host
      template:
        src: webapp-vhost.conf.j2
        dest: "/etc/nginx/sites-available/{{ app_name }}"
        owner: root
        group: root
        mode: '0644'
        backup: yes
      notify:
        - enable webapp vhost
        - validate nginx config
        - reload nginx
        - notify load balancers
    
    - name: Deploy SSL certificates
      copy:
        src: "{{ item.src }}"
        dest: "{{ item.dest }}"
        owner: root
        group: root
        mode: "{{ item.mode }}"
        backup: yes
      with_items:
        - { src: "ssl/{{ app_name }}.crt", dest: "/etc/ssl/certs/{{ app_name }}.crt", mode: "0644" }
        - { src: "ssl/{{ app_name }}.key", dest: "/etc/ssl/private/{{ app_name }}.key", mode: "0600" }
      notify:
        - validate ssl certificates
        - reload nginx
        - notify monitoring
    
    # Application Deployment
    - name: Deploy application configuration
      template:
        src: application.properties.j2
        dest: "/opt/{{ app_name }}/config/application.properties"
        owner: "{{ app_name }}"
        group: "{{ app_name }}"
        mode: '0644'
        backup: yes
      notify:
        - validate app config
        - restart application
        - wait for app startup
        - run health checks
        - notify monitoring
    
    - name: Deploy application binary
      copy:
        src: "{{ app_name }}-{{ app_version }}.jar"
        dest: "/opt/{{ app_name }}/lib/{{ app_name }}.jar"
        owner: "{{ app_name }}"
        group: "{{ app_name }}"
        mode: '0644'
        backup: yes
      notify:
        - restart application
        - wait for app startup
        - run health checks
        - notify load balancers
    
    # Load Balancer Configuration
    - name: Configure HAProxy
      template:
        src: haproxy.cfg.j2
        dest: /etc/haproxy/haproxy.cfg
        owner: root
        group: root
        mode: '0644'
        backup: yes
      notify:
        - validate haproxy config
        - restart haproxy
        - verify load balancer health
        - notify monitoring
    
    # Monitoring Configuration
    - name: Deploy monitoring agent configuration
      template:
        src: monitoring-agent.conf.j2
        dest: /etc/monitoring/agent.conf
        owner: monitoring
        group: monitoring
        mode: '0644'
        backup: yes
      notify:
        - restart monitoring agent
        - register with monitoring server
        - configure alerting rules
  
  handlers:
    # Database Handlers
    - name: restart mysql
      service:
        name: mysql
        state: restarted
      listen: "restart mysql"
    
    - name: wait for mysql
      wait_for:
        port: "{{ mysql_port }}"
        host: "{{ ansible_default_ipv4.address }}"
        delay: 10
        timeout: 60
      listen: "wait for mysql"
    
    - name: configure replication user
      mysql_user:
        name: "{{ replication_user }}"
        password: "{{ vault_replication_password }}"
        priv: "*.*:REPLICATION SLAVE"
        host: "%"
        state: present
      when: db_role == 'master'
      listen: "configure replication user"
    
    - name: configure slave replication
      mysql_replication:
        mode: changemaster
        master_host: "{{ groups['databases'] | select('match', '.*master.*') | first }}"
        master_user: "{{ replication_user }}"
        master_password: "{{ vault_replication_password }}"
        master_auto_position: yes
      when: db_role == 'slave'
      listen: "configure slave replication"
    
    # Cache Handlers
    - name: restart redis master
      service:
        name: redis-server
        state: restarted
      when: redis_role == 'master'
      listen: "restart redis master"
    
    - name: wait for redis master
      wait_for:
        port: "{{ redis_port }}"
        host: "{{ ansible_default_ipv4.address }}"
        delay: 5
        timeout: 30
      when: redis_role == 'master'
      listen: "wait for redis master"
    
    - name: notify redis slaves
      command: redis-cli -h {{ item }} -p {{ redis_port }} SLAVEOF {{ ansible_default_ipv4.address }} {{ redis_port }}
      with_items: "{{ groups['cache_servers'] | select('match', '.*slave.*') | list }}"
      when: redis_role == 'master'
      listen: "notify redis slaves"
    
    - name: restart redis slave
      service:
        name: redis-server
        state: restarted
      when: redis_role == 'slave'
      listen: "restart redis slave"
    
    - name: wait for redis slave
      wait_for:
        port: "{{ redis_port }}"
        host: "{{ ansible_default_ipv4.address }}"
        delay: 5
        timeout: 30
      when: redis_role == 'slave'
      listen: "wait for redis slave"
    
    - name: validate slave sync
      command: redis-cli -p {{ redis_port }} INFO replication
      register: redis_replication_info
      when: redis_role == 'slave'
      listen: "validate slave sync"
    
    - name: validate cache cluster
      command: redis-cli -p {{ redis_port }} PING
      register: redis_ping
      failed_when: redis_ping.stdout != 'PONG'
      listen: "validate cache cluster"
    
    # Web Server Handlers
    - name: validate nginx config
      command: nginx -t
      register: nginx_config_test
      failed_when: nginx_config_test.rc != 0
      listen: "validate nginx config"
    
    - name: reload nginx
      service:
        name: nginx
        state: reloaded
      listen: "reload nginx"
    
    - name: enable webapp vhost
      file:
        src: "/etc/nginx/sites-available/{{ app_name }}"
        dest: "/etc/nginx/sites-enabled/{{ app_name }}"
        state: link
      listen: "enable webapp vhost"
    
    - name: validate ssl certificates
      command: openssl x509 -in /etc/ssl/certs/{{ app_name }}.crt -text -noout
      register: ssl_cert_validation
      listen: "validate ssl certificates"
    
    # Application Handlers
    - name: validate app config
      command: java -jar /opt/{{ app_name }}/lib/{{ app_name }}.jar --validate-config
      become_user: "{{ app_name }}"
      register: app_config_validation
      failed_when: app_config_validation.rc != 0
      listen: "validate app config"
    
    - name: restart application
      service:
        name: "{{ app_name }}"
        state: restarted
      listen: "restart application"
    
    - name: wait for app startup
      wait_for:
        port: "{{ app_port }}"
        host: "{{ ansible_default_ipv4.address }}"
        delay: 15
        timeout: 120
      listen: "wait for app startup"
    
    - name: run health checks
      uri:
        url: "http://{{ ansible_default_ipv4.address }}:{{ app_port }}/health"
        method: GET
        status_code: 200
        timeout: 10
      register: health_check_result
      retries: 5
      delay: 10
      listen: "run health checks"
    
    # Load Balancer Handlers
    - name: validate haproxy config
      command: haproxy -f /etc/haproxy/haproxy.cfg -c
      register: haproxy_config_test
      failed_when: haproxy_config_test.rc != 0
      listen: "validate haproxy config"
    
    - name: restart haproxy
      service:
        name: haproxy
        state: restarted
      listen: "restart haproxy"
    
    - name: verify load balancer health
      uri:
        url: "http://{{ ansible_default_ipv4.address }}:8080/stats"
        method: GET
        status_code: 200
        timeout: 10
      listen: "verify load balancer health"
    
    # Cross-Service Handlers
    - name: notify load balancers
      uri:
        url: "http://{{ item }}:8080/admin/reload"
        method: POST
        status_code: 200
      with_items: "{{ groups['loadbalancers'] }}"
      when: inventory_hostname not in groups['loadbalancers']
      listen: "notify load balancers"
    
    - name: restart dependent services
      service:
        name: "{{ item }}"
        state: restarted
      with_items: "{{ service_start_order }}"
      when: item != 'mysql'
      listen: "restart dependent services"
    
    # Monitoring Handlers
    - name: restart monitoring agent
      service:
        name: monitoring-agent
        state: restarted
      listen: "restart monitoring agent"
    
    - name: register with monitoring server
      uri:
        url: "http://monitor.company.com/api/register"
        method: POST
        body_format: json
        body:
          hostname: "{{ inventory_hostname }}"
          ip: "{{ ansible_default_ipv4.address }}"
          services: "{{ ansible_facts.services.keys() | list }}"
        status_code: 201
      listen: "register with monitoring server"
    
    - name: configure alerting rules
      template:
        src: alerting-rules.yml.j2
        dest: "/etc/monitoring/rules/{{ inventory_hostname }}.yml"
        owner: monitoring
        group: monitoring
        mode: '0644'
      listen: "configure alerting rules"
    
    - name: notify monitoring
      command: >
        curl -X POST http://monitor.company.com/api/events
        -H 'Content-Type: application/json'
        -d '{"event": "deployment", "host": "{{ inventory_hostname }}", "timestamp": "{{ deployment_timestamp }}", "version": "{{ app_version }}"}'
      listen: "notify monitoring"
  
  post_tasks:
    - name: Verify all services are running
      service_facts:
      
    - name: Check critical services status
      assert:
        that:
          - ansible_facts.services[item + '.service'].state == 'running'
        fail_msg: "Critical service {{ item }} is not running"
      with_items: "{{ service_start_order }}"
      
    - name: Create deployment success marker
      file:
        path: "/opt/{{ app_name }}/deployment-{{ deployment_timestamp }}.success"
        state: touch
        owner: "{{ app_name }}"
        group: "{{ app_name }}"
        mode: '0644'
      
    - name: Clean up old deployment markers
      find:
        paths: "/opt/{{ app_name }}"
        patterns: "deployment-*.success"
        age: "30d"
      register: old_markers
      
    - name: Remove old deployment markers
      file:
        path: "{{ item.path }}"
        state: absent
      with_items: "{{ old_markers.files }}"
```

### Step 3: Create Rolling Deployment Example

3. Create `rolling_deployment.yml`:

```yaml
---
- name: Rolling Deployment with Handler Orchestration
  hosts: webservers
  become: yes
  serial: 1
  
  vars:
    deployment_strategy: "rolling"
    health_check_retries: 5
    health_check_delay: 30
  
  pre_tasks:
    - name: Remove server from load balancer
      uri:
        url: "http://{{ item }}:8080/admin/disable/{{ inventory_hostname }}"
        method: POST
        status_code: 200
      with_items: "{{ groups['loadbalancers'] }}"
      delegate_to: localhost
      
    - name: Wait for connections to drain
      wait_for:
        timeout: 60
      
  tasks:
    - name: Deploy new application version
      copy:
        src: "{{ app_name }}-{{ new_version }}.jar"
        dest: "/opt/{{ app_name }}/lib/{{ app_name }}.jar"
        owner: "{{ app_name }}"
        group: "{{ app_name }}"
        mode: '0644'
        backup: yes
      notify:
        - rolling restart application
        - rolling health check
        - add back to load balancer
  
  handlers:
    - name: rolling restart application
      service:
        name: "{{ app_name }}"
        state: restarted
      listen: "rolling restart application"
    
    - name: rolling health check
      uri:
        url: "http://{{ ansible_default_ipv4.address }}:{{ app_port }}/health"
        method: GET
        status_code: 200
        timeout: 10
      register: health_check
      retries: "{{ health_check_retries }}"
      delay: "{{ health_check_delay }}"
      listen: "rolling health check"
    
    - name: add back to load balancer
      uri:
        url: "http://{{ item }}:8080/admin/enable/{{ inventory_hostname }}"
        method: POST
        status_code: 200
      with_items: "{{ groups['loadbalancers'] }}"
      delegate_to: localhost
      listen: "add back to load balancer"
```

### Step 4: Run Enterprise Deployment

4. Deploy the enterprise application:

```bash
# Full enterprise deployment
ansible-playbook enterprise_deployment.yml -i enterprise_deployment_inventory.yml

# Rolling deployment
ansible-playbook rolling_deployment.yml -i enterprise_deployment_inventory.yml -e "new_version=3.2.2"

# Maintenance mode deployment
ansible-playbook enterprise_deployment.yml -i enterprise_deployment_inventory.yml -e "maintenance_mode=true"

# Deployment with custom batch size
ansible-playbook enterprise_deployment.yml -i enterprise_deployment_inventory.yml -e "deployment_batch_size=2"
```

### Key Production Features:

- **Service Orchestration**: Handlers coordinate complex service dependencies
- **Health Validation**: Comprehensive health checks at each deployment stage
- **Configuration Validation**: Validate configurations before applying changes
- **Rolling Deployments**: Zero-downtime deployments with load balancer integration
- **Backup and Rollback**: Automatic backup of configurations before changes
- **Cross-Service Communication**: Handlers notify dependent services of changes
- **Monitoring Integration**: Automatic registration and alerting configuration
- **Error Handling**: Comprehensive error handling and recovery procedures

---

## Summary

Handlers provide a powerful way to execute tasks in response to changes detected by other tasks. They are useful for cases where you want to restart a service after its configuration changes or, like in this lab, provide feedback once a certain task has completed successfully. The production example demonstrates enterprise-grade service orchestration with complex handler chains, health validation, and rolling deployment strategies. Mastering handlers can help you create more efficient and responsive playbooks. 👏