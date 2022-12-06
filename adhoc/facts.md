<link rel='stylesheet' href='../assets/css/main.css'/>

# Overview

In this lab, You will learn how to get facts from your hosts.

Basically, We are going to use a module called `setup`

## Duration

25 minutes

## Step 1 — getting to know the Module `setup`

`setup` module is used to pull config data from hosts

default template looks like:

```bash
ansible <Pattern> -m setup
```

**NOTE:** result of this module will be a lengthy JSON string.

## Step 2 - Running a sample

run the following command to see the results

```bash
ansible <Pattern> -m setup
```

Output will be long

## Step 3 — Filter Gathered Facts

To filter the received data from this module use the following template.

```bash
ansible <Pattern> -m setup -a "filter=<Your-needs>"
```

sample:

```bash
ansible webserver -m setup -a "filter=ansible_distribution*"
```

output

```console
<IP 1> | SUCCESS => {
    "ansible_facts": {
        "ansible_distribution": "Ubuntu",
        "ansible_distribution_file_parsed": true,
        "ansible_distribution_file_path": "/etc/os-release",
        "ansible_distribution_file_variety": "Debian",
        "ansible_distribution_major_version": "20",
        "ansible_distribution_release": "focal",
        "ansible_distribution_version": "20.04",
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false
}
<IP 2> | SUCCESS => {
    "ansible_facts": {
        "ansible_distribution": "Ubuntu",
        "ansible_distribution_file_parsed": true,
        "ansible_distribution_file_path": "/etc/os-release",
        "ansible_distribution_file_variety": "Debian",
        "ansible_distribution_major_version": "20",
        "ansible_distribution_release": "focal",
        "ansible_distribution_version": "20.04",
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false
}
```

As you can see, this command will give you the info about OS of the hosts.

Practice:

- Try to get time info from your hosts

**Hint:**

```bash
ansible webserver -m setup -a "filter=ansible_date_time*"
```

## Well done! 👏