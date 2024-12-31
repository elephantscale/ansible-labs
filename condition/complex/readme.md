# Using Advanced Conditionals in Ansible Playbook

## Duration

Approximately 20 minutes

## Objective

The goal of this lab is to deepen your understanding of using conditionals in Ansible playbooks. We will cover how to use complex conditions, combining multiple conditions using logical operators (`and`, `or`, `not`), and the usage of the `| bool` filter for more human-readable variables.

## Prerequisites:

- A foundational knowledge of Ansible.
- Ansible installed on your machine.
- Access to target hosts in your inventory.

## Step-by-step guide:

### Step A: Set Up the Playbook Structure

- Create a YAML file named `advanced_conditional_playbook.yml`.
- Start with:

```yaml
- name: A conditional play
  hosts: all
  gather_facts: no
```

### Step B: Define Variables

- Under the `vars` section, set up the following variables:

```yaml
  vars:
    epic: true
    boomer: no
    distro: "Ubuntu"
```

### Step C: Implement Conditional Tasks

- Add the task "Task A":

```yaml
    - name: Task A
      debug:
        msg: "Hello A"
      when: epic and boomer | bool
```

- Introduce the task "Task B":

```yaml
    - name: Task B
      debug:
        msg: "Hello B"
      when:
        - not epic
        - not boomer | bool
```

- Set up the task "Task C":

```yaml
    - name: Task C
      debug:
        msg: "Hello C"
      when: epic or distro == "Ubuntu" 
```

- Lastly, configure the task "Task D":

```yaml
    - name: Task D
      debug:
        msg: "Hello D"
      when:
        - epic
        - boomer | bool or distro == "Ubuntu"
```

### Step D: Execute the Playbook

- Save the `advanced_conditional_playbook.yml` file.
- Run the playbook:

```
ansible-playbook advanced_conditional_playbook.yml
```

## Final File

You can compare your playbook with the [advanced_conditional_playbook.yml](advanced_conditional_playbook.yml) file in the current directory.

## Summary

In this lab, you've delved deeper into the utilization of advanced conditionals in Ansible. By mastering these techniques, you can make your playbooks more dynamic and efficient, allowing for tailored execution based on the environment or system state.