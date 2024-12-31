# Looping Over External Variables with Ansible

## Duration

Approximately 30 minutes

## Objective

In this lab, you will understand how to use external variables with Ansible's `loop` directive. By leveraging external variables, playbooks can become more dynamic and reusable across different scenarios.

## Prerequisites:

- Ansible installed on your machine.
- A target host or set of hosts configured in your Ansible inventory.

## Step-by-step guide:

### Step A: Setting up the Playbook

- Begin by creating a new playbook. You can name it `loop_with_variables.yml`:

```yaml
---
- hosts: all
  name: Loop over external variables
  gather_facts: no

  vars:
    user_list:
      - Alice
      - Bob
      - Charlie
```

### Step B: Implementing the Loop

- Add a task to iterate over the `user_list` variable, greeting each user:

```yaml
  tasks:
    - name: Greeting each user
      debug:
        msg: "Hello, {{ item }}"
      loop: "{{ user_list }}"
```

In this step, the `loop` directive fetches names from the `user_list` variable and displays a greeting for each name.

### Step C: Running the Playbook

- Save the `loop_with_variables.yml` file.
- To run the playbook and see the iteration over external variables in action, execute:

```bash
ansible-playbook loop_with_variables.yml
```

- The output should display greetings for each user in the `user_list` variable.

## Final File

You can compare your playbook with the [loop_with_variables.yml](loop_with_variables.yml) file in the current directory.

## Summary

Using variables with Ansible's `loop` directive enhances playbook flexibility. Instead of hardcoding values, variables allow for easy modifications and adaptability across different environments or scenarios. With this knowledge, you can build more dynamic and reusable playbooks, improving
automation efficiency.