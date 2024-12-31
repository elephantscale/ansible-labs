# Using the Assert Module with Input in Ansible

## Duration

Approximately 20 minutes

## Objective

In this lab, you'll delve deeper into the `assert` module, learning to validate user input. By prompting the user for input and then asserting against the provided value, you'll ensure your playbooks remain both interactive and foolproof.

## Prerequisites:

- A foundational knowledge of Ansible.
- Ansible installed on your machine.

## Step-by-step guide:

### Step A: Setting up the Playbook

- Start by creating a new playbook named `assert_input_example.yml` with the basic structure:

```yaml
---
- name: Demonstrate Assert with User Input
  hosts: localhost
  gather_facts: no
```

### Step B: Prompt for User Input

- Add a `vars_prompt` section to gather a number from the user:

```yaml
  vars_prompt:
    - name: "user_number"
      prompt: "Please enter a number between 1 and 10"
      private: no
```

### Step C: Assert the User Input

- Use the `assert` module to ensure that the number entered by the user is within the specified range:

```yaml
  tasks:
    - name: Assert that the number is between 1 and 10
      assert:
        that:
          - user_number | int >= 1
          - user_number | int <= 10
        fail_msg: "The entered number is not between 1 and 10"
        success_msg: "The entered number is valid. Proceeding..."
```

### Step D: Display a Success Message

- For demonstration purposes, add a debug task to show a success message:

```yaml
    - name: Display a success message
      debug:
        msg: "You've successfully entered a valid number. Thank you!"
```

### Step E: Execute the Playbook

- Save the `assert_input_example.yml` file.
- Run the playbook:

```bash
ansible-playbook assert_input_example.yml
```

- Follow the on-screen prompt and enter a number. If you provide a number outside the range 1-10, the playbook will halt with an assertion failure message.

## Final File

You can compare your playbook with the [assert_input_example.yml](assert_input_example.yml) file in the current directory.

## Summary

Utilizing the `assert` module with user input in Ansible allows for dynamic playbooks that can validate and react according to provided values. This approach ensures playbooks remain adaptive, interactive, and resilient to undesired inputs. Armed with this knowledge, you can now build even more interactive and foolproof playbooks.