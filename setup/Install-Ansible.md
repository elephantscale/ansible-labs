<link rel="stylesheet" href="../assets/css/main.css"/>

# Lab: Installing Ansible on Your Machine

## Overview

In this lab, you will learn how to install Ansible on your designated machine (`ansible-machine`).

## Duration

**Estimated Time:** 20 minutes

---

## Step 1: Log In to the Ansible Machine

1. Open **Google Chrome** on your computer.
2. Navigate to the `<Ansible Lab>` link provided by your instructor. We’ll refer to it as `ansible-machine`.
3. Click on the option labeled **Secure Access via HTTPS (Recommended)**.
4. Enter the provided password to log in and access the desktop environment.
5. Open a terminal window.

---

## Step 2: Verify Ansible Installation

1. In the terminal, check if Ansible is already installed by running the following command:

```bash
ansible --version
```

2. If Ansible is not installed, you will see a response indicating that the command is not recognized.

---

## Step 3: Install Ansible

1. Update the system packages:

```bash
sudo apt update && sudo apt upgrade -y
```

2. Install the required dependencies:

```bash
sudo apt install software-properties-common -y
```

3. Add the official Ansible PPA repository:

```bash
sudo add-apt-repository --yes --update ppa:ansible/ansible
```

4. Install Ansible:

```bash
sudo apt install ansible -y
```

5. Verify the installation by checking the Ansible version:

```bash
ansible --version
```

You should see output similar to the following:

```console
ansible [core 2.18.1]
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/home/ubuntu/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 3.10.12 (main, Nov 20 2023, 15:14:05) [GCC 11.4.0]
```

**Note:** The version number and Python version may vary depending on your system. As of 2025, you should see Ansible 8.x+ or 9.x+ versions with ansible-core 2.15+ or higher.


## Step 4: Install `nano`

To keep commands as simple as possible, we'll use `nano`

```bash
sudo apt install nano -y
```

---

## Conclusion

Congratulations! 🎉 You have successfully installed Ansible on your `ansible-machine`.

---
