# Error Handling with Ansible Blocks, Rescue, and Always

## Duration

Approximately 30 minutes

## Objective

In this lab, you will explore the powerful error handling features provided by Ansible's `block`, `rescue`, and `always` directives. By deliberately introducing failures, you'll gain firsthand experience with how Ansible can gracefully handle errors and execute specific tasks based on the success or failure of previous tasks.

## Prerequisites:

- Ansible installed on your machine.
- A target host or set of hosts configured in your Ansible inventory under the group `appservers`.

## Step-by-step guide:

### Step A: Setting up the Playbook

- Begin by creating a new playbook named `error_handling_demo.yml`.
- Define the playbook structure:

```yaml
---
- hosts: appservers
  become: yes
  become_user: root
  gather_facts: yes
```

### Step B: Defining the `block` Directive

- Within the `tasks` section, define a `block` that encapsulates a series of tasks. Tasks within this block will execute sequentially:

```yaml
  tasks:
    - name: Attempt and graceful roll back demo
      block:
        - name: Print a message
          debug:
            msg: 'I execute normally'
```

### Step C: Introduce a Failure

- After the debug message, introduce a deliberate failure using the `command` module:

```yaml
        - name: Force a failure
          command: /bin/false
```

### Step D: Adding Additional Tasks

- Following the failure, add a task that will never execute due to the prior failure:

```yaml
        - name: Never print this
          debug:
            msg: 'I never execute, due to the above task failing, :-('
```

### Step E: Using the `rescue` Directive

- After the `block` section, define a `rescue` section. This section will execute tasks only if there's a failure within the `block`:

```yaml
      rescue:
        - name: Print when errors
          debug:
            msg: 'I caught an error'
```

### Step F: Add a Deliberate Failure in the `rescue` Section

- After the debug message, introduce another deliberate failure:

```yaml
        - name: Force a failure in middle of recovery! >:-)
          command: /bin/false
```

### Step G: Adding Additional Tasks in the `rescue` Section

- Following the failure, add another task that, again, will never execute due to the prior failure:

```yaml
        - name: Never print this
          debug:
            msg: 'I also never execute :-('
```

### Step H: Using the `always` Directive

- Finally, define an `always` section. This section will always execute, regardless of the success or failure of the `block` and `rescue` sections:

```yaml
      always:
        - name: Always do this
          debug:
            msg: "This always executes"
```

### Step I: Running the Playbook

- Save the `error_handling_demo.yml` file.
- Execute the playbook using:

```bash
ansible-playbook error_handling_demo.yml
```

Observe the flow of the playbook execution. The `block` section will fail, triggering the `rescue` section. When the `rescue` section also encounters a failure, it will skip its subsequent tasks. Finally, the `always` section will execute.

## Final File

You can compare your playbook with the [error_handling_demo.yml](error_handling_demo.yml) file in the current directory.

## Summary

The `block`, `rescue`, and `always` directives offer a robust mechanism for error handling and cleanup tasks in Ansible. They enable you to group tasks, handle errors gracefully, and ensure certain tasks always execute, even in the face of errors. Understanding these directives is vital for writing resilient Ansible playbooks.