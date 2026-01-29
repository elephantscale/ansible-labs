# Lab Instructions: Using Conditionals in Ansible Playbooks

## Overview

In this lab, you will learn how to use conditionals in an Ansible playbook. Specifically, you will understand how the `when` clause can be used to control the execution of tasks based on the value of a variable.

---

## Objectives

- Learn how to use the `when` clause to implement conditionals.
- Create a task that executes only under specific conditions.

---

## Prerequisites

- Basic understanding of Ansible playbooks.
- Ansible installed on your ansible machine.
- Access to target hosts in your inventory.

---

## Duration

**Estimated Time:** 20 minutes

---

## Instructions

### Step 1: Set Up the Ansible Playbook

1. Create a new YAML file named `conditional_playbook.yml`.

2. Define the play's name, target hosts (`all` in this case), and whether to gather facts:

   ```yaml
   ---
   - name: A conditional play
     hosts: all
     gather_facts: no
   ```

   **Explanation:**
   - `hosts: all` targets all hosts in the inventory.
   - `gather_facts: no` skips automatic fact gathering to keep the playbook simple.

---

### Step 2: Define a Variable

1. In the `vars` section, define a variable named `epic` and assign it a boolean value:

   ```yaml
     vars:
       epic: true
   ```

   **Explanation:**
   - The variable `epic` will be used to control the execution of tasks.

---

### Step 3: Add a Conditional Task

1. Under the `tasks` section, add a task that prints a message "Hello A" if the variable `epic` is `true`:

   ```yaml
     tasks:
       - name: Task A
         debug:
           msg: "Hello A"
         when: epic
   ```

   **Explanation:**
   - The `when` clause ensures the task runs only when the condition (`epic: true`) is met.

---

### Step 4: Execute the Playbook

1. Save the `conditional_playbook.yml` file.

2. Run the playbook using the following command:

   ```bash
   ansible-playbook conditional_playbook.yml
   ```

3. Observe the output. The task "Task A" will execute only if `epic` is set to `true`.

---

## Conclusion

In this lab, you've learned how to use conditionals in Ansible playbooks, progressing from simple boolean checks to complex production-ready logic.

**What you learned:**
- Basic `when` clause usage
- OS-specific conditional logic
- Environment-based configurations
- Complex multi-condition logic
- Real-world business logic implementation

**Key Conditional Patterns:**
- Single condition: `when: variable`
- Multiple conditions: `when: condition1 and condition2`
- List conditions: `when: variable in ["value1", "value2"]`
- Complex logic: `when: (condition1 or condition2) and condition3`

These patterns form the foundation for building sophisticated, environment-aware automation! 👏

