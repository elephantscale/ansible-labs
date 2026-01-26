# Lab: Configure Inventory

## Overview

In this lab, you will create a group and add a machine to that group. This involves creating an `Inventory` file for Ansible.

---

## Duration

**Estimated Time:** 25 minutes

---

## Step 1: Familiarizing Yourself with Ansible

Ensure that `ansible` is installed on your system.

Navigate to the Ansible directory:

```bash
cd /etc/ansible

# To list the files in the directory
ls
```

**Output:**

```console
ansible.cfg  hosts
```

- **`ansible.cfg`**: This is the Ansible configuration file.
- **`hosts`**: This file is used to store the inventory of your hosts and their configurations.

**Notes:** 
- The `hosts` file is also known as the inventory file.
- There could be another files and directories in the `/etc/ansible` directory, but for this lab, we are only interested in the `hosts` file.

---

## Step 2: Creating a Group

Add a new group to the `/etc/ansible/hosts` file. To do this, open the file for editing:

```bash
sudo nano /etc/ansible/hosts
```

At the end of the file, add the following group:

```console
[webservers]
```

### Notes:
- Group names are **case-sensitive** and must be enclosed in square brackets `[]`.
- Group names must be **unique**.
- Avoid spaces or special characters in group names.
- Use meaningful names that are easy to remember for future labs.

Keep the file open and proceed to the next step.

---

## Step 3: Adding Hosts

Add the designated server(s) to your group in the `hosts` file. For example:

```console
[webservers]
<IP Machine 1>  ansible_ssh_pass=<Machine 1 Password>
```

Keep the file open and continue to the next step.

---

## Step 4: Defining Username and Password for Hosts

Since Ansible uses SSH to connect to hosts, you need to specify the username and password. This is done using the `:vars` syntax.

For the `[webservers]` group, add `[webservers:vars]` after the last host entry:

### Variables:
- **`ansible_user`**: The username to use for SSH.
- **`ansible_ssh_pass`**: The password to use for SSH (for lab purposes only).

**Security Note:** In production environments, always use SSH keys instead of passwords for authentication.

### Example:

```console
[webservers]
<IP Machine 1>  ansible_ssh_pass=<Machine 1 Password>

[webservers:vars]
ansible_user=<Username>  # Example: azureuser in this lab environment
ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```

### Production Alternative (Recommended):
For production environments, use SSH keys instead:

```console
[webservers]
<IP Machine 1>

[webservers:vars]
ansible_user=<Username>
ansible_ssh_private_key_file=/path/to/your/private/key
```

Save the file by pressing `Ctrl+O` (then Enter), and exit by pressing `Ctrl+X`.

---

## Well Done! 👏

You have successfully created a group, added a host, and defined its connection details in the Ansible inventory!
We will use this inventory in the upcoming labs to run playbooks on the hosts you have defined.

