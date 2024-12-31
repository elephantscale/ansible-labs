# Challenge: Using Conditionals and Variable Prompts in Ansible Playbook

## Duration

Approximately 30 minutes

## Objective

The purpose of this challenge is to test your understanding of using conditionals, variable prompts, and task execution based on the values of these variables in an Ansible playbook.

## Prerequisites:

- A foundational knowledge of Ansible.
- Ansible installed on your machine.
- Access to target hosts in your inventory, particularly those categorized as "webservers."

## Step-by-step guide:

### Step A: Set Up the Playbook Structure

- Create a YAML file named `conditional_prompt_challenge.yml`.
- Start with:

```yaml
- name: Conditional play
  gather_facts: no
  hosts: webservers
```

### Step B: Implement Variable Prompt

- Add a `vars_prompt` to solicit user input for a variable called `question`. The input should be either "yes" or "no".

```yaml
  vars_prompt:
  # TODO: Prompt for a value for question variable, it should be yes/no (Student task)
```

### Step C: Define Static Variables

- Provide the following pre-defined variables:

```yaml
  vars:
    epic: true
    distro: "Ubuntu"
```

### Step D: Add Task Conditionals

- Add a task to validate that the `question` variable holds a valid value, either "yes" or "no".

```yaml
    #TODO: Add validation for question variable (Student task)
```

- Construct the first task. It should only execute if the `question` variable holds the value of "yes" and the `distro` is "Ubuntu":

```yaml
    - name: "Print if question is true and distro is Ubuntu" #TODO: (Student task)
      when: #?
      debug:
        msg: "Task 1"
```

- Implement the second task. This task should be executed if the `question` variable holds the value "no" or if `epic` is true:

```yaml
    - name: "print if question is false or epic is true" #TODO: (Student task)
      when: #?
      debug:
        msg: "Task 2"
```

### Step E: Test the Playbook

- Save the `conditional_prompt_challenge.yml` file.
- Execute the playbook:

```
ansible-playbook conditional_prompt_challenge.yml
```

## Final File

The Template of the challenge can be found [Here](conditional_prompt_challenge.yml) file in the current directory.

## Solution

You can compare your solution with either of these files:

- [Solution 1](conditional_prompt_challenge_solution_1.yml)
- [Solution 2](conditional_prompt_challenge_solution_2.yml)
- [Solution with assert](conditional_prompt_challenge_solution_3.yml)

## Summary

Through this challenge, you practiced constructing Ansible playbooks that prompt users for input, validate this input, and then use it, along with other variables, to dictate task execution. It's a crucial skill when creating dynamic and adaptable automation scripts using Ansible.