# Iterating Over Items with Ansible

## Duration

Approximately 20 minutes

## Objective

In this lab, you will learn how to use the `with_items` directive in Ansible to iterate over a list of items and execute tasks for each item. By the end, you'll be familiar with iterating over a list in a playbook and displaying a customized message for each list item.

## Prerequisites:

- Ansible installed on your machine.
- Access to a target server named "webserver" in your Ansible inventory.

## Step-by-step guide:

### Step A: Setting up the Playbook

- Start by creating a new playbook. Let's name it `item_iteration.yml` with the given structure:

```yaml
---
- hosts: webserver
  become: yes
  name: Print a list
```

### Step B: Adding the Iteration Task

- Add a task to iterate over a list of names and print a custom message for each:

```yaml
  tasks:
    - name: Print a list
      debug:
        msg: "Hello, {{ item }}"
      with_items:
        - John
        - Jane
        - Alex
        - sarah
```

### Step C: Running the Playbook

- Save the `item_iteration.yml` file.
- To execute the playbook and see the list iteration in action, run:

```bash
ansible-playbook item_iteration.yml
```

- Observe the output; for each name in the list, Ansible will print a custom message greeting the name.

## Final File

You can compare your playbook with the [item_iteration.yml](item_iteration.yml) file in the current directory.

## Summary

The `with_items` directive in Ansible provides a powerful way to iterate over lists and execute tasks multiple times with different data. This approach is incredibly useful for repetitive tasks and operations that need to be executed on multiple items, making your Ansible playbooks more dynamic and
efficient. With the knowledge from this lab, you can now apply list iteration to various other tasks in your Ansible playbooks.