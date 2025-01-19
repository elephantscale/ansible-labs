# Lab: File and Folder Manipulations

## Overview

In this lab, you will learn how to create, copy, and delete files and folders on your hosts using the `file` module in Ansible.

---

## Duration

**Estimated Time:** 30 minutes

---

## Step 1: Understanding the `file` Module

The `file` module in Ansible is used to manipulate files and folders on your hosts. This module requires several arguments:

- **`src`**: The source path of your file/folder on the Control Node (C&C).
- **`dest`**: The destination path where the file/folder will be placed on your hosts.
- **`owner`**: The owner user on the hosts (optional).
- **`group`**: The owner group on the hosts (optional).
- **`state`**: Specifies whether to remove or create a folder/file on the hosts.
- **`mode`**: Sets permissions on your files (optional).

The default command template is:

```bash
ansible <Pattern> -m file -a "src=<FULL-PATH> dest=<Host-FULL-PATH> state=<absent|directory|file>"
```

---

## Step 2: Copying a File

1. Create a file on the Control Node:

    ```bash
    cd ~
    touch hello.txt
    nano hello.txt
    ```

    Write `Hello World` in `hello.txt` and save the file.

2. Copy the file to the `webserver` group:

    ```bash
    ansible webservers -m copy -a "src=~/hello.txt dest=~"
    ```

    Check the output for a `CHANGED` status, indicating that something on your hosts has changed.

3. Log in to each host and verify that the file is there.

---

## Step 3: Deleting a File

To delete a file on your hosts, use the following command:

```bash
ansible webservers -m file -a "dest=~/hello.txt state=absent"
```

After running this command, log in to each host and verify that the file is deleted.

---

## Step 4: Creating a Directory

1. Create a directory and a file within it on the Control Node:

    ```bash
    cd ~
    mkdir test
    cd test
    touch hello.txt
    nano hello.txt
    ```

    Write `Hello File` in the file, save, and exit.

2. Create the directory on the hosts:

    ```bash
    ansible webservers -m file -a "dest=~/test state=directory"
    ```

3. Copy the file to the directory on the hosts:

    ```bash
    ansible webservers -m copy -a "src=~/test/ dest=~/test"
    ```

    Log in to each host to verify the presence of the folder and file.

---

## Step 5: Deleting a Directory

Delete a specific directory from all hosts in the `webserver` group:

```bash
ansible webservers -m file -a "dest=~/test state=absent"
```

---

## Step 6: Practice

Practice your skills by:

1. Creating a directory `~/<Your-Name>/<Your-Family>`.
2. Creating two `txt` files inside the directory with your email and name.
3. Using Ansible to push these files to your hosts.

---

## Conclusion

Well done! You've successfully learned how to manage files and directories on your hosts using Ansible. 👏

