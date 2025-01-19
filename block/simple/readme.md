# Lab: Error Handling with Ansible Blocks, Rescue, and Always

## Overview

In this lab, you will explore the powerful error-handling features provided by Ansible's `block`, `rescue`, and `always` directives. By deliberately introducing failures, you'll gain firsthand experience with how Ansible can gracefully handle errors and execute specific tasks based on the success or failure of previous tasks.

---

## Objectives

- Use the `block` directive to group related tasks.
- Leverage the `rescue` directive to handle errors and provide recovery steps.
- Utilize the `always` directive to ensure cleanup or follow-up tasks always execute.

---

## Prerequisites

- Ansible installed on your ansible machine.
- A target host or set of hosts configured in your Ansible inventory under the group `appservers`.

---

## Duration

**Estimated Time:** 30 minutes

---

## Instructions

### Step 1: Set Up the Playbook

1. Create a new playbook named `error_handling_demo.yml`.

2. Define the play structure:

   ```yaml
   ---
   - name: Error Handling Demo
     hosts: appservers
     become: yes
     become_user: root
     gather_facts: yes
   ```

---

### Step 2: Define a `block` Section

1. Under the `tasks` section, define a `block` that encapsulates a series of tasks to execute sequentially:

   ```yaml
     tasks:
       - name: Error handling demonstration
         block:
           - name: Print a success message
             debug:
               msg: "This task executes normally."
   ```

---

### Step 3: Introduce a Failure

1. After the debug message, add a deliberate failure using the `command` module:

   ```yaml
           - name: Introduce a failure
             command: /bin/false
   ```

2. Add a task that will not execute due to the failure:

   ```yaml
           - name: This task will be skipped
             debug:
               msg: "This will not execute due to the previous failure."
   ```

---

### Step 4: Add a `rescue` Section

1. Add a `rescue` section to handle the error from the `block`:

   ```yaml
         rescue:
           - name: Handle the error
             debug:
               msg: "An error occurred. Executing rescue tasks."
   ```

2. Optionally, add another deliberate failure in the `rescue` section:

   ```yaml
           - name: Deliberate failure in rescue
             command: /bin/false
   ```

3. Add a task that will not execute due to the rescue failure:

   ```yaml
           - name: This will also be skipped
             debug:
               msg: "This will not execute due to the rescue failure."
   ```

---

### Step 5: Add an `always` Section

1. Define an `always` section to include tasks that should execute regardless of success or failure:

   ```yaml
         always:
           - name: Execute cleanup tasks
             debug:
               msg: "This task always executes."
   ```

---

### Step 6: Execute the Playbook

1. Save the `error_handling_demo.yml` file.

2. Run the playbook using:

   ```bash
   ansible-playbook error_handling_demo.yml
   ```

3. Observe the output. The `block` section will fail, triggering the `rescue` section. When the `rescue` section also fails, its subsequent tasks will be skipped. Finally, the `always` section will execute.

---

## Solution File

You can compare your playbook with the [error_handling_demo.yml](error_handling_demo.yml) file in the current directory.

---

## Conclusion

The `block`, `rescue`, and `always` directives provide a robust mechanism for error handling and cleanup in Ansible. These directives help you manage errors gracefully and ensure critical tasks always execute, even in the face of failures. Mastering these techniques will make your playbooks more resilient and reliable. 👏

