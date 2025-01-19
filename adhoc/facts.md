# Lab: Gathering Facts with Ansible

## Overview

In this beginner-friendly lab, you'll learn how to gather information (facts) from your hosts using Ansible. We'll focus on a module called `setup`, which is essential for collecting configuration data.

---

## Duration

**Estimated Time:** 25 minutes

---

## Step 1: Understanding the `setup` Module

The `setup` module in Ansible is used to gather configuration data from your hosts. It's a powerful tool for understanding the state and settings of your systems.

Here's the basic command format:

```bash
ansible <Pattern> -m setup
```

**Note:** The output of this command is a detailed JSON string, which contains a lot of information.

---

## Step 2: Running a Sample Command

Let's see this module in action. Run the following command:

```bash
ansible webservers -m setup
```

The output will be extensive, showing a lot of details about your host's configuration.

---

## Step 3: Filtering Gathered Facts

Often, you'll only need specific pieces of information. To narrow down the results, use a filter with your command:

```bash
ansible webservers -m setup -a "filter=<Your-Desired-Info>"
```

For example, to find out about the operating system of your hosts, you can run:

```bash
ansible webserver -m setup -a "filter=ansible_distribution*"
```

### Expected Output:

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
```

This command provides detailed information about the operating system of your hosts.

---

### Practice Exercise

Try to gather time-related information from your hosts:

**Hint:**

```bash
ansible webservers -m setup -a "filter=ansible_date_time*"
```

---

## Conclusion

Great job! You've learned how to use the `setup` module in Ansible to gather facts about your hosts. This skill is fundamental for managing and automating tasks in your network. Keep practicing to become more familiar with Ansible's capabilities. 👏

