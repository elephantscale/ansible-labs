# Lab Title Here

**Using Conditionals with Boolean Filters in Ansible Playbook**

## Duration

Approximately 20 minutes

## Objective

The focus of this lab is to understand how to use conditionals with boolean filters in an Ansible playbook. You will see how to leverage the `| bool` filter to evaluate variables that are expressed in a more human-friendly manner, like "yes" or "no", and then determine the execution of tasks based
on the boolean value.

## Prerequisites:

- Basic understanding of Ansible.
- Ansible installed on your machine.
- Access to target hosts in your inventory.

## Step-by-step guide:

### Step A: Set up the Ansible Playbook

- Create a new YAML file named `conditional_bool_playbook.yml`.
- Begin by defining the play's name, the target hosts (`all` in this context), and opting out of gathering facts.

```yaml
- name: A conditional play
  hosts: all
  gather_facts: no
```

### Step B: Define the Variable

- Under the `vars` section, set up a variable named `boomer`. Here, the value "no" equates to `false`, and conversely, "yes" would equate to `true`.

```yaml
  vars:
    boomer: no # no = false = 0  , yes = true = 1
```

### Step C: Add a Conditional Task

4. Introduce a task named "Task A". This task will display the message "Hello A". The task's execution is governed by the truthiness of the `boomer` variable, which is evaluated using the `| bool` filter.

```yaml
  tasks:
    - name: Task A
      debug:
        msg: "Hello A"
      when: boomer | bool # if boomer is true, then run this task
```

### Step D: Run the Playbook

- Store your changes to the `conditional_bool_playbook.yml` file.
- To execute the playbook, type:

```
ansible-playbook conditional_bool_playbook.yml
```

## Final File

You can compare the playbook you created with the [conditional_bool_playbook.yml](conditional_bool_playbook.yml) file in the present directory.

## Summary

Through this lab, you've been introduced to the concept of boolean filters in Ansible. With the `| bool` filter, Ansible enables you to evaluate more human-centric values like "yes" or "no" as true or false. This provides flexibility and clarity in your playbooks, ensuring tasks run under the
desired conditions.