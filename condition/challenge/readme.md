# Challenge: Using Conditionals and Variable Prompts in Ansible Playbook

## Overview

This challenge will test your ability to use conditionals, variable prompts, and task execution based on variable values in an Ansible playbook. By constructing a playbook that incorporates these elements, you will enhance your skills in creating dynamic and interactive automation scripts.

---

## Objectives

- Prompt the user for input using `vars_prompt`.
- Validate and use the input with conditionals.
- Execute tasks based on logical conditions.

---

## Prerequisites

- Basic knowledge of Ansible playbooks.
- Ansible installed on your ansible machine.
- Access to target hosts categorized as "webservers."

---

## Duration

**Estimated Time:** 30 minutes

---

## Instructions

### Step 1: Set Up the Playbook Structure

1. Create a YAML file named `conditional_prompt_challenge.yml`.

2. Define the play's name, target hosts, and skip fact gathering:

   ```yaml
   ---
   - name: Conditional play
     hosts: webservers
     gather_facts: no
   ```

---

### Step 2: Implement Variable Prompt

1. Add a `vars_prompt` section to solicit user input for a variable named `question`. The input should be either "yes" or "no":

   ```yaml
     vars_prompt:
       - name: "question"
         prompt: "Please answer 'yes' or 'no'"
         private: no
   ```

---

### Step 3: Define Static Variables

1. Add the following pre-defined variables:

   ```yaml
     vars:
       epic: true
       distro: "Ubuntu"
   ```

   **Explanation:**
   - `epic`: A boolean variable.
   - `distro`: Represents the operating system.

---

### Step 4: Add Conditional Tasks

1. Add a task to validate that the `question` variable holds a valid value:

   ```yaml
       - name: Validate user input
         assert:
           that:
             - question in ["yes", "no"]
           fail_msg: "Invalid input. Please enter 'yes' or 'no'."
   ```

2. Add the first task to execute if `question` is "yes" and `distro` is "Ubuntu":

   ```yaml
       - name: Print message if question is 'yes' and distro is Ubuntu
         debug:
           msg: "Condition met: question is 'yes' and distro is Ubuntu."
         when: question == "yes" and distro == "Ubuntu"
   ```

3. Add the second task to execute if `question` is "no" or `epic` is true:

   ```yaml
       - name: Print message if question is 'no' or epic is true
         debug:
           msg: "Condition met: question is 'no' or epic is true."
         when: question == "no" or epic
   ```

---

### Step 5: Test the Playbook

1. Save the `conditional_prompt_challenge.yml` file.

2. Run the playbook using the following command:

   ```bash
   ansible-playbook conditional_prompt_challenge.yml
   ```

3. Provide input when prompted and observe the task execution based on the conditions.

---

## Solution Files

You can compare your solution with the provided examples:

- [Solution 1](conditional_prompt_challenge_solution_1.yml)
- [Solution 2](conditional_prompt_challenge_solution_2.yml)
- [Solution with assert](conditional_prompt_challenge_solution_3.yml)

---

## Conclusion

In this challenge, you practiced creating Ansible playbooks that prompt users for input, validate the input, and execute tasks based on logical conditions. Mastering these techniques is essential for creating dynamic and adaptable automation scripts in Ansible. Keep refining your skills to handle more complex scenarios! 👏

