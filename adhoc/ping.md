# Lab: Ping Hosts

## Overview

In this lab, you will learn how to confirm the connection between your control node and hosts.

We will use the `ping` module for this purpose.

---

## Duration

**Estimated Time:** 20 minutes

---

## Step 1: Understanding the `ping` Module

The `ping` module is used to verify connectivity between Ansible and its hosts.

The default syntax for using the `ping` module is:

```bash
ansible <Pattern> -m ping
```

---

## Step 2: Running a Sample Command

Run the following command to test the connection:

```bash
ansible webservers -m ping
```

### Expected Output:

```console
<IP 1> | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

### Key Points:
- **`SUCCESS`**: Indicates that Ansible successfully connected to the hosts.
- **`changed: false`**: Confirms that no changes were made on the hosts.
- **`ping: "pong"`**: Confirms connectivity between Ansible and the hosts.

**Note:** This is not an ICMP ping; it is a test of Ansible's ability to connect to the hosts.

---

## Well Done! 👏

You have successfully verified connectivity using the `ping` module!

