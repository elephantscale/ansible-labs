# Looping Over Complex Data Structures with Ansible

## Duration

Approximately 20 minutes

## Objective

In this lab, you will learn how to use Ansible's `loop` directive along with `loop_control` to iterate over a list of dictionaries. This method is more powerful and flexible than the older `with_items` directive. By the end of this lab, you'll have a clear understanding of how to handle complex loop scenarios using a custom loop variable.

## Prerequisites:

- Ansible installed on your machine.
- A target host or set of hosts configured in your Ansible inventory.

## Step-by-step guide:

### Step A: Setting up the Playbook

- Start by creating a new playbook. You can name it `loop_complex_data.yml` with the given structure:

```yaml
---
- hosts: all
  name: Items play
  gather_facts: no
```

### Step B: Adding the Loop with Complex Data

- Add a task to iterate over a list of dictionaries, each containing IP and gateway details:

```yaml
  tasks:
    - name: Print a List of names
      loop_control: 
        loop_var: networkInfo
      debug:
        msg: "Hello {{ networkInfo.ip }} can be anything {{ networkInfo.gateway }}"      
      loop:
        - { ip: "10.10.10.2", gateway: "10.10.10.1" }
        - { ip: "10.10.11.2", gateway: "10.10.11.1" }
        - { ip: "10.10.12.2", gateway: "10.10.12.1" }
```

Here, the `loop` directive is used to iterate over the list of dictionaries. Using `loop_control`, we can assign a custom variable name `networkInfo` for each item in the list. This makes accessing the dictionary values clearer and more readable.

### Step C: Running the Playbook

- Save the `loop_complex_data.yml` file.
- To execute the playbook and observe the iteration over complex data in action, run:

```bash
ansible-playbook loop_complex_data.yml
```

- You should see a custom message for each dictionary item in the list, displaying the IP and gateway values.

## Final File

You can compare your playbook with the [loop_complex_data.yml](loop_complex_data.yml) file in the current directory.

## Summary

Ansible's `loop` directive, especially when combined with `loop_control`, offers a robust mechanism for iterating over complex data structures. This method simplifies the playbook and improves readability when dealing with more intricate looping scenarios. With the skills acquired from this lab, you can implement loops in more advanced situations, giving your Ansible playbooks added versatility and clarity.