# Overview

This callback plugin writes playbook output to a file per host in the `/var/log/ansible/hosts` directory.


## Duration

10 minutes

## Create Log directory

Use the following command to create the log directory with 777 permission.

```
sudo mkdir -m 777 /var/log/ansible/
```


## Ansible Config file

To enable this plugin, we need to add `log_plays` to the `defaults` section of the `ansible.cfg` file.

* Open config file with sudo access

```
sudo nano /etc/ansible/ansible.cfg
```

* Add the following line to the `defaults` section:

```
stdout_callback = log_plays
```

NOTE: By enabling this plugin, you will not see the result on terminal. This is useful for cron jobs.



## Well done! 👏