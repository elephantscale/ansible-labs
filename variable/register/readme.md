# Mastering the `register` Keyword in Ansible with File Creation

## Duration

30 minutes

## Objective

Learn to use the `register` keyword in Ansible to capture and utilize the output of commands or modules in subsequent tasks, including creating a file on the target machine.

## Prerequisites

- Basic Ansible knowledge.
- Ansible installed on your system.
- Access to the `webservers` group in your inventory.

## Steps

### Step 1: Create the Playbook

- Make a new file `register_playbook.yml`.
- Define the play with the following content:

    ```yaml
    - name: Demonstrate register usage with file creation
      hosts: webservers
      gather_facts: no
    ```

### Step 2: Create a File on the Target Machine

- Add a task to create a file named `a.txt` in `/home/ubuntu/`.

    ```yaml
      tasks:
        - name: Create a test file
          copy:
            content: "Sample text for testing."
            dest: /home/ubuntu/a.txt
    ```

### Step 3: Capture Command Output

- Add a task to read `/home/ubuntu/a.txt` using the `shell` module.
- Use `register` to save the output to `motd_contents`.
- Note: Use a separate playbook for this step.

    ```yaml
        - name: Capture file content
          shell: cat /home/ubuntu/a.txt
          register: motd_contents
    ```

### Step 4: Display the Captured Content

- Add a task to display the content using the `debug` module.

    ```yaml
        - name: Display file content
          debug:
            msg: "{{ motd_contents.stdout }}"
    ```

### Step 5: Run the Playbook

- Save your playbook.
- Execute it with:

    ```
    ansible-playbook register_playbook.yml
    ```

## Review

Your final playbook should now include a step to create `a.txt` on the target machine, then read and display its contents. This lab demonstrates how `register` can be effectively used in Ansible to
capture and reuse output, enhancing playbook flexibility and capability.