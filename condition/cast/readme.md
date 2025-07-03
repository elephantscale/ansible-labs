# Lab: Using Boolean Filters in Ansible Playbooks

## Overview

In this lab, you will learn how to use boolean filters in Ansible playbooks. Specifically, you will understand how the `| bool` filter can evaluate human-friendly variable values like "yes" or "no" into boolean `true` or `false`, and use them to control task execution.

---

## Objectives

- Learn how to use the `| bool` filter to evaluate variables.
- Create tasks that execute conditionally based on boolean filters.

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

1. Create a new YAML file named `conditional_bool_playbook.yml`.

2. Define the play's name, target hosts (`all` in this case), and whether to gather facts:

   ```yaml
   ---
   - name: A conditional play with boolean filters
     hosts: all
     gather_facts: no
   ```

   **Explanation:**
   - `hosts: all` targets all hosts in the inventory.
   - `gather_facts: no` skips automatic fact gathering for simplicity.

---

### Step 2: Define a Variable

1. In the `vars` section, define a variable named `boomer` and assign it the value `no`:

   ```yaml
     vars:
       boomer: no # "no" translates to false, "yes" translates to true
   ```

   **Explanation:**
   - The variable `boomer` is a human-readable string that will be evaluated as a boolean.

---

### Step 3: Add a Conditional Task

1. Under the `tasks` section, add a task that prints a message "Hello A" if the `boomer` variable evaluates to `true`:

   ```yaml
     tasks:
       - name: Task A
         debug:
           msg: "Hello A"
         when: boomer | bool
   ```

   **Explanation:**
   - The `when: boomer | bool` clause ensures the task runs only if `boomer` evaluates to `true` (e.g., "yes").

---

### Step 4: Execute the Playbook

1. Save the `conditional_bool_playbook.yml` file.

2. Run the playbook using the following command:

   ```bash
   ansible-playbook conditional_bool_playbook.yml
   ```

3. Observe the output. The task "Task A" will execute only if `boomer` evaluates to `true`.

---

---

## Production Example: Feature Flag Management System

### Overview
This production example demonstrates how to implement a sophisticated feature flag system using boolean filters to control application features across different environments and user segments.

### Step 1: Create Feature Flag Inventory

1. Create an inventory file `feature_flags_inventory.yml`:

```yaml
all:
  children:
    production:
      hosts:
        prod-web-01:
          ansible_host: 10.0.1.10
        prod-web-02:
          ansible_host: 10.0.1.11
      vars:
        environment: production
        # Production feature flags - conservative approach
        enable_new_ui: "no"
        enable_beta_features: "no"
        enable_debug_mode: "no"
        enable_analytics: "yes"
        enable_caching: "yes"
        enable_monitoring: "yes"
        
    staging:
      hosts:
        staging-web-01:
          ansible_host: 10.0.2.10
      vars:
        environment: staging
        # Staging feature flags - test new features
        enable_new_ui: "yes"
        enable_beta_features: "yes"
        enable_debug_mode: "yes"
        enable_analytics: "yes"
        enable_caching: "yes"
        enable_monitoring: "yes"
        
    canary:
      hosts:
        canary-web-01:
          ansible_host: 10.0.3.10
      vars:
        environment: canary
        # Canary release - selective feature rollout
        enable_new_ui: "yes"
        enable_beta_features: "no"
        enable_debug_mode: "no"
        enable_analytics: "yes"
        enable_caching: "yes"
        enable_monitoring: "yes"
        canary_percentage: 10
```

### Step 2: Create Feature Flag Configuration Template

2. Create `templates/feature_flags.conf.j2`:

```jinja2
# Feature Flags Configuration - {{ environment.upper() }}
# Generated on {{ ansible_date_time.iso8601 }}

[ui_features]
{% if enable_new_ui | bool %}
new_ui_enabled = true
ui_theme = modern
dashboard_v2 = true
{% else %}
new_ui_enabled = false
ui_theme = classic
dashboard_v2 = false
{% endif %}

[beta_features]
{% if enable_beta_features | bool %}
experimental_api = true
advanced_search = true
ml_recommendations = true
{% else %}
experimental_api = false
advanced_search = false
ml_recommendations = false
{% endif %}

[debugging]
{% if enable_debug_mode | bool %}
debug_level = DEBUG
stack_traces = true
performance_metrics = true
{% else %}
debug_level = ERROR
stack_traces = false
performance_metrics = false
{% endif %}

[analytics]
{% if enable_analytics | bool %}
tracking_enabled = true
user_behavior_analysis = true
conversion_tracking = true
{% else %}
tracking_enabled = false
user_behavior_analysis = false
conversion_tracking = false
{% endif %}

[performance]
{% if enable_caching | bool %}
redis_cache = true
memory_cache = true
cdn_enabled = true
{% else %}
redis_cache = false
memory_cache = false
cdn_enabled = false
{% endif %}

[monitoring]
{% if enable_monitoring | bool %}
health_checks = true
metrics_collection = true
alerting = true
{% else %}
health_checks = false
metrics_collection = false
alerting = false
{% endif %}

{% if canary_percentage is defined %}
[canary]
rollout_percentage = {{ canary_percentage }}
{% endif %}
```

### Step 3: Create Production Feature Flag Playbook

3. Create `production_feature_flags.yml`:

```yaml
---
- name: Production Feature Flag Management
  hosts: all
  become: yes
  gather_facts: yes
  
  vars:
    # Default feature flags (can be overridden per environment)
    default_features:
      enable_new_ui: "no"
      enable_beta_features: "no"
      enable_debug_mode: "no"
      enable_analytics: "yes"
      enable_caching: "yes"
      enable_monitoring: "yes"
    
    # Application settings
    app_name: "webapp"
    config_path: "/etc/{{ app_name }}"
    
  pre_tasks:
    - name: Validate feature flag variables
      assert:
        that:
          - enable_new_ui is defined
          - enable_beta_features is defined
          - enable_debug_mode is defined
          - enable_analytics is defined
          - enable_caching is defined
          - enable_monitoring is defined
        fail_msg: "Required feature flag variables are missing"
    
    - name: Display current feature flag status
      debug:
        msg: |
          Environment: {{ environment }}
          New UI: {{ enable_new_ui | bool }}
          Beta Features: {{ enable_beta_features | bool }}
          Debug Mode: {{ enable_debug_mode | bool }}
          Analytics: {{ enable_analytics | bool }}
          Caching: {{ enable_caching | bool }}
          Monitoring: {{ enable_monitoring | bool }}
  
  tasks:
    - name: Create application config directory
      file:
        path: "{{ config_path }}"
        state: directory
        owner: "{{ app_name }}"
        group: "{{ app_name }}"
        mode: '0755'
    
    - name: Deploy feature flags configuration
      template:
        src: feature_flags.conf.j2
        dest: "{{ config_path }}/feature_flags.conf"
        owner: "{{ app_name }}"
        group: "{{ app_name }}"
        mode: '0644'
        backup: yes
      notify: restart application
    
    # Conditional tasks based on feature flags
    - name: Install Redis for caching
      package:
        name: redis-server
        state: present
      when: enable_caching | bool
      notify: start redis
    
    - name: Configure Redis
      template:
        src: redis.conf.j2
        dest: /etc/redis/redis.conf
        backup: yes
      when: enable_caching | bool
      notify: restart redis
    
    - name: Install monitoring agent
      package:
        name: "{{ item }}"
        state: present
      loop:
        - collectd
        - telegraf
      when: enable_monitoring | bool
      notify: start monitoring
    
    - name: Configure monitoring
      template:
        src: monitoring.conf.j2
        dest: /etc/monitoring/config.conf
      when: enable_monitoring | bool
      notify: restart monitoring
    
    - name: Install debug tools
      package:
        name: "{{ item }}"
        state: present
      loop:
        - strace
        - tcpdump
        - htop
      when: enable_debug_mode | bool
    
    - name: Configure log level for debugging
      lineinfile:
        path: "{{ config_path }}/logging.conf"
        regexp: '^log_level='
        line: "log_level={{ 'DEBUG' if enable_debug_mode | bool else 'INFO' }}"
        create: yes
      notify: restart application
    
    - name: Deploy analytics tracking script
      template:
        src: analytics.js.j2
        dest: /var/www/html/js/analytics.js
        owner: www-data
        group: www-data
        mode: '0644'
      when: enable_analytics | bool
    
    - name: Remove analytics tracking script
      file:
        path: /var/www/html/js/analytics.js
        state: absent
      when: not (enable_analytics | bool)
    
    - name: Create feature flag audit log
      lineinfile:
        path: /var/log/feature-flags.log
        line: "{{ ansible_date_time.iso8601 }} - {{ environment }} - {{ inventory_hostname }} - Features: UI={{ enable_new_ui | bool }}, Beta={{ enable_beta_features | bool }}, Debug={{ enable_debug_mode | bool }}, Analytics={{ enable_analytics | bool }}, Cache={{ enable_caching | bool }}, Monitor={{ enable_monitoring | bool }}"
        create: yes
  
  handlers:
    - name: restart application
      service:
        name: "{{ app_name }}"
        state: restarted
    
    - name: start redis
      service:
        name: redis-server
        state: started
        enabled: yes
    
    - name: restart redis
      service:
        name: redis-server
        state: restarted
    
    - name: start monitoring
      service:
        name: "{{ item }}"
        state: started
        enabled: yes
      loop:
        - collectd
        - telegraf
    
    - name: restart monitoring
      service:
        name: "{{ item }}"
        state: restarted
      loop:
        - collectd
        - telegraf
```

### Step 4: Create Advanced Conditional Logic

4. Create `advanced_conditionals.yml`:

```yaml
---
- name: Advanced Feature Flag Conditionals
  hosts: all
  gather_facts: yes
  
  vars:
    # Complex feature flag logic
    user_segments:
      - name: "premium"
        percentage: 100
        features: ["new_ui", "beta_features", "analytics"]
      - name: "standard"
        percentage: 50
        features: ["new_ui", "analytics"]
      - name: "free"
        percentage: 0
        features: ["analytics"]
  
  tasks:
    - name: Set complex feature flags based on conditions
      set_fact:
        enable_feature_x: "{{ 'yes' if (
          (environment == 'production' and enable_new_ui | bool) or
          (environment == 'staging') or
          (canary_percentage is defined and canary_percentage | int > 50)
        ) else 'no' }}"
    
    - name: Enable features based on time conditions
      set_fact:
        enable_time_sensitive_feature: "{{ 'yes' if (
          ansible_date_time.hour | int >= 9 and 
          ansible_date_time.hour | int <= 17 and
          ansible_date_time.weekday | int < 5
        ) else 'no' }}"
    
    - name: Complex conditional task execution
      debug:
        msg: "Feature X is enabled during business hours"
      when: 
        - enable_feature_x | bool
        - enable_time_sensitive_feature | bool
        - environment != 'production' or enable_monitoring | bool
    
    - name: Multi-condition feature deployment
      template:
        src: advanced_feature.conf.j2
        dest: /etc/app/advanced_feature.conf
      when: >
        (enable_beta_features | bool and environment in ['staging', 'canary']) or
        (enable_new_ui | bool and enable_analytics | bool and environment == 'production')
```

### Step 5: Run Production Feature Flag Management

5. Deploy feature flags to different environments:

```bash
# Deploy to production (conservative flags)
ansible-playbook production_feature_flags.yml -i feature_flags_inventory.yml -l production

# Deploy to staging (enable new features)
ansible-playbook production_feature_flags.yml -i feature_flags_inventory.yml -l staging

# Deploy to canary with custom percentage
ansible-playbook production_feature_flags.yml -i feature_flags_inventory.yml -l canary -e "canary_percentage=25"

# Override specific feature for production
ansible-playbook production_feature_flags.yml -i feature_flags_inventory.yml -l production -e "enable_new_ui=yes"
```

### Key Production Features:

- **Environment-Based Feature Control**: Different flags per environment
- **Conditional Service Management**: Install/configure services based on flags
- **Audit Logging**: Track feature flag changes and deployments
- **Complex Boolean Logic**: Multi-condition feature enablement
- **Time-Based Conditions**: Enable features during specific hours
- **Rollback Capability**: Backup configurations before changes
- **Resource Management**: Install dependencies only when needed

---

## Conclusion

In this lab, you've learned how to use boolean filters in Ansible playbooks to evaluate variables like "yes" or "no" into boolean values. By leveraging the `| bool` filter, you can make your playbooks more adaptable and intuitive. The production example demonstrates enterprise-grade feature flag management with complex conditional logic, environment-specific controls, and automated service management. Experiment with other human-friendly values and conditions to further enhance your automation skills! 👏

