# Lab: Using the Assert Module with Input in Ansible

## Overview

In this lab, you will learn how to use the `assert` module in Ansible to validate user input. By prompting the user for input and asserting conditions against the provided values, you can ensure your playbooks are interactive and error-proof.

---

## Objectives

- Learn how to use the `assert` module to validate input.
- Prompt for user input using `vars_prompt`.
- Ensure playbook tasks execute only when valid inputs are provided.

---

## Prerequisites

- Basic understanding of Ansible playbooks.
- Ansible installed on your ansible machine.

---

## Duration

**Estimated Time:** 20 minutes

---

## Instructions

### Step 1: Set Up the Ansible Playbook

1. Create a new YAML file named `assert_input_example.yml`.

2. Define the play's name, target hosts (`localhost` in this case), and whether to gather facts:

   ```yaml
   ---
   - name: Demonstrate Assert with User Input
     hosts: localhost
     gather_facts: no
   ```

---

### Step 2: Prompt for User Input

1. Add a `vars_prompt` section to collect a number from the user:

   ```yaml
     vars_prompt:
       - name: "user_number"
         prompt: "Please enter a number between 1 and 10"
         private: no
   ```

   **Explanation:**
   - `vars_prompt` prompts the user for input when the playbook is executed.
   - The input is stored in the `user_number` variable.

---

### Step 3: Assert the User Input

1. Add a task to validate that the number entered by the user is within the range 1 to 10:

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

   **Explanation:**
   - The `assert` module validates conditions defined in the `that` list.
   - If the conditions fail, the playbook halts and displays the `fail_msg`.
   - If the conditions pass, the `success_msg` is shown.

---

### Step 4: Display a Success Message

1. Add a task to display a message confirming successful input validation:

   ```yaml
       - name: Display a success message
         debug:
           msg: "You've successfully entered a valid number. Thank you!"
   ```

   **Explanation:**
   - The `debug` module displays a custom success message if the input passes validation.

---

### Step 5: Execute the Playbook

1. Save the `assert_input_example.yml` file.

2. Run the playbook using the following command:

   ```bash
   ansible-playbook assert_input_example.yml
   ```

3. Follow the prompt to enter a number. If the number is outside the range 1-10, the playbook will halt with an assertion failure message.

---

## Conclusion

In this lab, you've learned how to use the `assert` module in Ansible to validate user input dynamically. This approach ensures your playbooks remain interactive, adaptable, and resilient to incorrect inputs. By incorporating input validation, you can create more reliable and user-friendly playbooks. Keep experimenting to further enhance your automation workflows! 👏

