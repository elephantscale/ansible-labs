# Lab: Using Loop with Include_Tasks in Ansible

## Overview

In this lab, you will learn how to leverage Ansible's `loop` directive in conjunction with the `include_tasks` feature to create a more modular and efficient playbook structure. By iterating over a list of file-related configurations, you can include tasks from an external file to handle repetitive file operations dynamically.

---

## Objectives

- Use the `loop` directive to iterate over a list of dictionaries.
- Include external tasks using the `include_tasks` module.
- Modularize playbooks for cleaner and reusable structures.

---

## Prerequisites

- Ansible installed on your ansible machine.
- A target host or set of hosts configured in your Ansible inventory.
- Familiarity with basic Ansible file modules like `file` and `copy`.

---

## Duration

**Estimated Time:** 30 minutes

---

## Instructions

### Step 1: Set Up the Main Playbook

1. Create a new playbook named `loop_include_playbook.yml`.

2. Define the play structure and include a variable for file configurations:

   ```yaml
   ---
   - name: Loop Play
     hosts: all
     gather_facts: no

     vars:
       filesInfo:
         - { item_src: "a.txt", item_dest_path: "/home/ubuntu/farshid", item_dest_file: "a.ini" }
         - { item_src: "b.txt", item_dest_path: "/home/ubuntu/sam", item_dest_file: "b.ini" }
         - { item_src: "c.txt", item_dest_path: "/home/ubuntu/tim", item_dest_file: "c.ini" }
   ```

   **Explanation:**
   - `filesInfo`: A list of dictionaries containing file source paths, destination directories, and file names.

---

### Step 2: Add a Task to Include External Tasks with a Loop

1. Under the `tasks` section, add a task to iterate over `filesInfo` and include external tasks:

   ```yaml
     tasks:
       - name: Include external tasks with loop
         include_tasks:
           file: loop_task.yaml
         loop: "{{ filesInfo }}"
         loop_control:
           loop_var: file
   ```

   **Explanation:**
   - `include_tasks`: Dynamically includes tasks from an external file.
   - `loop`: Iterates over the `filesInfo` list.
   - `loop_control.loop_var`: Assigns a custom loop variable (`file`) for better readability within the included tasks.

---

### Step 3: Create the External Task File

1. Create a file named `loop_task.yaml` in the same directory as your main playbook.

2. Add the following tasks to handle debugging, directory creation, and file copying:

   ```yaml
   ---
   - debug:
       msg: "Processing file: {{ file }}"

   - name: Create destination directory
     file:
       dest: "{{ file.item_dest_path }}"
       state: directory

   - name: Copy file to destination
     copy:
       src: "{{ file.item_src }}"
       dest: "{{ file.item_dest_path }}/{{ file.item_dest_file }}"
   ```

   **Explanation:**
   - `debug`: Prints the current dictionary being processed.
   - `file`: Creates the destination directory if it doesn’t exist.
   - `copy`: Copies the file from the source to the specified destination.

---

### Step 4: Execute the Playbook

1. Save both `loop_include_playbook.yml` and `loop_task.yaml`.

2. Run the playbook using the following command:

   ```bash
   ansible-playbook loop_include_playbook.yml
   ```

3. Ensure that the source files (`a.txt`, `b.txt`, `c.txt`) exist in your Ansible machine directory, or adjust the `item_src` paths accordingly.

4. Observe the output. The playbook will process each dictionary in the `filesInfo` list, dynamically including tasks from `loop_task.yaml` for each iteration.

---

## Solution Files

You can compare your playbook and task file with the following examples:

- [loop_include_playbook.yml](loop_include_playbook.yml)
- [loop_task.yaml](loop_task.yaml)

---

## Conclusion

By combining Ansible's `loop` directive with `include_tasks`, you can create modular, clean, and efficient playbooks. This approach is particularly useful for managing repetitive tasks across varying configurations or environments. With this technique, you can streamline your playbook structure and improve reusability, making your automation workflows more dynamic and maintainable! 👏

