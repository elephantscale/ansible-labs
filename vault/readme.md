# Secure Data with Ansible Vault

## Duration

45 minutes

## Objective

In this hands-on lab, you will master the art of securing sensitive data using Ansible Vault, ensuring that critical information like passwords remains confidential.

## Prerequisites:

- Ansible installed on your machine.
- A target host or set of hosts configured in your Ansible inventory under the group `webservers`.

## Step-by-step guide:

### Step 1: Initiate a Vault-secured File

To start, let's create an encrypted file. This can be a brand-new file created during the vaulting process or an existing file that needs encryption. Let's delve into the former first:

Run the following command:

```bash
$ ansible-vault create test-vault.yml
```

Here, we've conceived a new file named `test-vault.yml`.

After entering some crucial data, save it.

On inspecting the file:

```bash
$ cat test.yml
```

You'll notice its content has undergone encryption.

To peek inside, execute:

```bash
$ ansible-vault view test-vault.yml
```

You'll be asked for a password, post which the file's original content will be displayed.

### Step 2: Encrypting Pre-existing Files

Imagine having a file, `vars.yml`, already brimming with passwords and sensitive information:

```text
db_user: "roger"
db_ssh_pass: "cisco"
db_network_os: "ios" 
```

Now, let's encrypt this existing trove:

```bash
$ ansible-vault encrypt vars.yml
```

On being prompted, provide a password. Voilà! The file's cloaked.

To double-check, view the file:

```bash
$ cat vars.yml
```

The gibberish you see confirms encryption.

### Step 3: Integrating Ansible Vault within a Playbook

Time to draft a playbook, `play.yml`, which requires data from `vars.yml`.

On firing up the playbook:

```text
Error: Attempting to decrypt but no vault secrets found
```

Oops! The hitch? `vars.yml` is encrypted and demands a password. Ansible's in the dark here.

There are multiple solutions:

- Provide the password when prompted during the playbook's execution.
- During playbook execution, indicate the path to a password file.
- In `ansible.cfg`, denote the password file's path.

Let's explore these:

#### Option 1: Live Password Prompt during Playbook Execution

Append `--ask-vault-pass` to your playbook command:

```bash
$ ansible-playbook play.yml --ask-vault-pass 
```

#### Option 2: Referencing a Password File

You might prefer storing your password in a distinct file. Remember, this file's confidentiality is paramount; secure it using machine-specific permissions and refrain from Git-pushing it.

To guide Ansible towards it, utilize:

```bash
$ ansible-playbook play.yml --vault-password-file vault-pass.txt
```

#### Option 3: Directing Ansible via `ansible.cfg`

Within `ansible.cfg`, indicate the password file's location:

```text
vault_password_file = vault-pass.txt
```

#### Step 4: Unmasking Encrypted Files

If encryption's no longer desired, decrypt with ease:

```bash
$ ansible-vault decrypt <filename> 
```
## Final File

You can compare your playbook with the [play.yaml](play.yaml) file in the current directory.

You can compare your variables with the [vars.yaml](vars.yaml) file in the current directory.


## Summary

You've successfully navigated the intricacies of Ansible Vault, ensuring your data's security. Keep vaulting!