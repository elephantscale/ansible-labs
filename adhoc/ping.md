# Overview

In this lab, You will learn how to confirm the connection between your control center and hosts.

Basically, We are going to use a module called `ping` 


## Duration

25 minutes

## Step 1 — getting to know the Module `ping`

`ping` module is used to verify connectivity between the control center and hosts.

default template looks like:

```bash
$ ansible <Pattern> -m ping
```

## Step 2 - Running a sample

Run the following command to see the results

```bash
$ ansible webserver -m ping
```

output

```console
<IP 1> | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

Note the following part
```console
    SUCCESS
    "changed": false,
    "ping": "pong"
```

This means
- Ansible can connect to the hosts
- Nothing is changed on the hosts


## Well done! 👏