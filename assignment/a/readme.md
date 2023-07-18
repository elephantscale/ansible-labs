# Assignment: Multi-OS User and Package Management with Ansible

The objective of this assignment is to create an Ansible playbook that performs user and package management on multiple systems.

## Tasks:

1. Identify the operating system of the target machines.

2. If the machine is a Debian-based system, perform the following tasks:
    - Create a user named 'student'.
    - Set the password for 'student' to 'password123'.
    - Update all packages.
    - Install the 'tree', 'vim', and 'curl' packages.
    - If any error occurs during these tasks, print an error message.

3. If the machine is a RedHat-based system, perform the following tasks:
    - Create a user named 'student'.
    - Set the password for 'student' to 'password123'.
    - Update all packages.
    - Install the 'httpd', 'vim-enhanced', and 'wget' packages.
    - If any error occurs during these tasks, print an error message.

## Hints:

- Use the `user` module to manage user accounts.
- Use the `apt` module for package management on Debian systems, and the `yum` module on RedHat systems.
- Use the `ansible_facts['os_family']` variable to identify the OS family.
- Use blocks to group related tasks together.
- Use the `rescue` section to handle any potential errors during the execution of the block.
- Use the `openssl_password` lookup plugin to generate a hashed password for the user.

The skeleton playbook can be here: [assignment.yaml](assignment.yaml)

