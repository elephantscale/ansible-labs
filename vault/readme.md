# Lab: Secure Data with Ansible Vault

## Overview

In this lab, you will master the art of securing sensitive data using Ansible Vault, ensuring that critical information like passwords remains confidential and protected from unauthorized access.

---

## Objectives

- Learn to create and manage encrypted files using Ansible Vault.
- Understand how to use encrypted variables in playbooks.
- Explore different methods for providing Vault passwords during playbook execution.
- Practice decrypting files securely.

---

## Prerequisites

- Ansible installed on your machine.
- A target host or set of hosts configured in your Ansible inventory under the group `webservers`.

---

## Duration

**Estimated Time:** 45 minutes

---

## Instructions

### Step 1: Create a Vault-Secured File

1. To create a new encrypted file, use the following command:

   ```bash
   ansible-vault create test-vault.yml
   ```

   - This command creates a new file named `test-vault.yml` and prompts you to set a Vault password.
   - Add sensitive data to the file and save it.

2. Verify the encryption by viewing the file:

   ```bash
   cat test-vault.yml
   ```

   - The content will be encrypted and unreadable.

3. To view the original content, use:

   ```bash
   ansible-vault view test-vault.yml
   ```

   - Enter the Vault password to decrypt and display the file content.

---

### Step 2: Encrypt an Existing File

1. If you have a file named `vars.yml` with sensitive information:

   ```yaml
   db_user: "admin"
   db_password: "securepassword"
   ```

2. Encrypt the file using the following command:

   ```bash
   ansible-vault encrypt vars.yml
   ```

   - Set a Vault password when prompted.

3. Verify the encryption:

   ```bash
   cat vars.yml
   ```

   - The content will now appear as encrypted text.

---

### Step 3: Use Vault in a Playbook

1. Create a playbook named `secure_playbook.yml` to use encrypted variables:

   ```yaml
   ---
   - name: Use Vault in a playbook
     hosts: webservers
     vars_files:
       - vars.yml
     tasks:
       - name: Display database credentials
         debug:
           msg: "Database user: {{ db_user }} and password: {{ db_password }}"
   ```

2. Run the playbook and provide the Vault password when prompted:

   ```bash
   ansible-playbook secure_playbook.yml --ask-vault-pass
   ```

   - This method securely decrypts and uses the variables in the playbook.

---

### Step 4: Provide Vault Password Automatically

1. Save the Vault password in a secure file named `vault-pass.txt`:

   ```
   echo 'your_vault_password' > vault-pass.txt
   chmod 600 vault-pass.txt
   ```

2. Use the `--vault-password-file` option to provide the password during playbook execution:

   ```bash
   ansible-playbook secure_playbook.yml --vault-password-file vault-pass.txt
   ```

3. Alternatively, configure the Vault password file in `ansible.cfg`:

   ```ini
   [defaults]
   vault_password_file = vault-pass.txt
   ```

   - This eliminates the need to specify the password file in the command.

---

### Step 5: Decrypt a File

1. To remove encryption from a file, use the following command:

   ```bash
   ansible-vault decrypt vars.yml
   ```

   - Enter the Vault password to decrypt the file.

---

## Solution Files

You can compare your playbook with the following files in the current directory:

- [secure_playbook.yml](secure_playbook.yml)
- [vars.yml](vars.yml)

---

## Conclusion

This lab demonstrated how to use Ansible Vault to secure sensitive data effectively. By encrypting variables and files, you can ensure critical information remains confidential while maintaining flexibility in your automation workflows. Mastering Vault is essential for creating secure and robust playbooks. 👏

