# Using Conditionals in Ansible Playbook

## Duration

Approximately 20 minutes

## Objective

In this lab, you will learn how to use conditionals in an Ansible playbook. Specifically, you will understand how the `when` clause can be used to control the execution of tasks based on the value of a variable.

## Prerequisites:

- Basic understanding of Ansible.
- Ansible installed on your machine.
- Access to target hosts in your inventory.

## Step-by-step guide:

### Step A: Set up the Ansible Playbook

- Create a new YAML file named `conditional_playbook.yml`.
- Specify the play's name, target hosts (`all` in this case), and the decision to avoid gathering facts.

```yaml
- name: A conditional play
  hosts: all
  gather_facts: no
```

### Step B: Define a Variable

In the `vars` section, define a variable named `epic` and assign it the boolean value `true`.

```yaml
  vars:
    epic: true
```

### Step C: Add a Conditional Task

Add a task named "Task A" that prints a message "Hello A". This task will only execute if the variable `epic` is set to `true`.

```yaml
  tasks:
    - name: Task A
      debug:
        msg: "Hello A"
      when: epic 
```

### Step D: Execute the Playbook

- Save the `conditional_playbook.yml` file.
- Run the playbook using the following command:

```
ansible-playbook conditional_playbook.yml
```

## Final File

You can compare your playbook with the [conditional_playbook.yml](conditional_playbook.yml) file in the current directory.

## Summary

In this lab, you've learned how to utilize conditionals in an Ansible playbook. By leveraging the `when` clause, you can make the execution of specific tasks contingent on certain conditions, thus making your playbooks more dynamic and adaptive.