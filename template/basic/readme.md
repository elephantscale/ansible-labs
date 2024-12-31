# Deploying a Configuration File using Templates in Ansible

## Duration

Approximately 20 minutes

## Objective

In this lab, participants will familiarize themselves with how Ansible uses the `template` module to deploy configuration files on servers. The lab emphasizes the importance of separating configuration data from tasks, allowing for dynamic content generation.

## Prerequisites:

- Ansible installed on the participant's machine.
- A basic understanding of Ansible playbooks and templates.
- A target host or set of hosts categorized as `webservers` in your Ansible inventory.
- A template file named `config.ini.jn2` in your working directory.

## Step-by-step guide:

### Step A: Setting up the Playbook

- Create a new playbook named `config_deployment_demo.yml`.

### Step B: Define Play Variables

- In your playbook, define the necessary database configurations under the `vars` section:

```yaml
vars:
  dbUsername: root
  dbPassword: mySecurePassword
```

### Step C: Template Configuration

- Ensure you have the template file `config.ini.jn2` with the following content:

```
db_username = {{ dbUsername }}
db_pass = {{ dbPassword }}
```

This template uses Jinja2 syntax to embed the values of `dbUsername` and `dbPassword` into the configuration file.

### Step D: Deploy the Configuration

- In your playbook, create a task to deploy the configuration using the `template` module:

```yaml
- name: Send Config to servers
  template:
    src: config.ini.jn2
    dest: ~/config.ini
```

This task will replace the placeholders in the `config.ini.jn2` file with the actual values defined in the `vars` section and deploy it to the target servers.

### Step E: Running the Playbook

- Save the `config_deployment_demo.yml` file.
- Execute the playbook:

```bash
ansible-playbook config_deployment_demo.yml
```

Monitor the output to ensure that the configuration is successfully sent to the servers.

## Final File

You can compare your playbook with the [config_deployment_demo.yml](config_deployment_demo.yml) file in the current directory.

## Summary

This lab allowed participants to understand the power of Ansible templates in deploying configuration files. Utilizing templates in this manner ensures consistent configurations across environments while allowing for dynamic value substitutions.