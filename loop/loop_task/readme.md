# Using Loop with Include_Tasks in Ansible

## Duration

Approximately 30 minutes

## Objective

In this lab, you will learn how to leverage Ansible's `loop` directive in conjunction with the `include_tasks` feature to create a more modular and efficient playbook structure. You will loop over a list of file-related configurations and include tasks from an external file to handle file-related
operations.

## Prerequisites:

- Ansible installed on your machine.
- A target host or set of hosts configured in your Ansible inventory.
- Familiarity with basic Ansible file modules like `file` and `copy`.

## Step-by-step guide:

### Step A: Setting up the Main Playbook

- Create a new playbook named `loop_include_playbook.yml`.
- Set the playbook structure as provided:

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

### Step B: Implementing the Loop with External Task Inclusion

- In the same playbook, add a task to iterate over `filesInfo` and include external tasks from `ch_task.yaml`:

```yaml
  tasks:
    - name: include external tasks with loop
      include_tasks:
        file: loop_task.yaml
      loop: "{{ filesInfo }}"
      loop_control:
        loop_var: file
```

This configuration allows you to loop over `filesInfo` and use the `file` variable to provide data to the tasks included from `ch_task.yaml`.

### Step C: Creating the External Task File

- Now, create a file named `loop_task.yaml` in the same directory.
- Add the following tasks to handle debugging, directory creation, and file copying:

```yaml
---
- debug:
    msg: "{{ file }}"

- name: create dir
  file:
    dest: "{{ file.item_dest_path }}"
    state: directory

- name: copy files
  copy:
    src: "{{ file.item_src }}"
    dest: "{{ file.item_dest_path }}/{{ file.item_dest_file }}"
```

### Step D: Running the Playbook

- Save both `loop_include_playbook.yml` and `loop_task.yaml`.
- To execute the playbook and observe how the external tasks are included and iterated over, run:

```bash
ansible-playbook loop_include_playbook.yml
```

- Ensure that the `a.txt`, `b.txt`, and `c.txt` files exist in your Ansible control machine directory, or adjust the source paths as needed.

## Final File

You can compare your playbook and task file with the [loop_include_playbook.yml](loop_include_playbook.yml) and [loop_task.yaml](loop_task.yaml) files in the current directory.

## Summary

By combining the power of Ansible's `loop` directive with `include_tasks`, you can create more modular, clean, and organized playbooks. This method is particularly helpful for managing repetitive tasks across varying configurations or environments. The skills you acquired from this lab enable you to
streamline playbook operations and improve your Ansible practices.