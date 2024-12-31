# Overview

In this lab, You will create two groups and add two machines to each group.  

Basically, We are going to create an `Inventory` 


## Duration

25 minutes


## Step 1 — Knowing your ansible

At this point, you should have `ansible` installed. 

Go to `ansible` directory

```bash
$ cd /etc/ansible

# to get a list of files
$ ls 
```

output

```console
ansible.cfg  hosts
```

`ansible.cfg` is your configuration file.

`hosts` is where will store our hosts and their configurations

## Step 2 - Install `nano`

To keep commands as simple as possible, we'll use `nano`

```bash
$ sudo apt-get install nano -y
```


## Confirm passwordless ssh (optional)


You should be able to `ssh` to localhost, as follows:

```bash
ssh localhost  # you may have to type in yes when asking for a key
exit
```

If this does not work, you need to configure the key for `passwordless` ssh to localhost.

```bash
ssh-keygen -t rsa # Press enter for each line
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod og-wx ~/.ssh/authorized_keys
```


## Step 3 — Add Groups

We need to add our new hosts to our `/etc/ansible/hosts` file

```bash
$ sudo nano /etc/ansible/hosts
```

Create the following groups by adding them to the last lines.

```console
[webservers]

[appserver]
```

Keep the file open and go to the next step

## Step 4 — Add Hosts

Add the designated servers to each group. 

Your file should look like:


```console
[webserver]
<IP 1>
<IP 2>

[appserver]
<IP 3>
<IP 4>
```
Keep the file open and go to the next step

## Step 4 — Define a username and password for Hosts

Since `ansible` relies on `ssh` to connect to host, we need to tell ansible what `username` and `password` it should use.

Syntax is: `[GroupName:vars]`

For `[webserver]` add `[webserver:vars]` after the last host

Variable for username is: `ansible_user`  
Variable for password is: `ansible_ssh_pass`

output
```console
[GroupName:vars]
ansible_user=<Username>
ansible_ssh_pass=<Password>
```

Now, what if each host has a unique password?
In this scenario `ansible_ssh_pass` will be placed in front of each host with a space

```console
[webserver]
<IP 1>  ansible_ssh_pass=<Password>
<IP 2>  ansible_ssh_pass=<Password>

[webserver:vars]
ansible_user=<Username> # ( ubuntu in our environment )

[appserver]
<IP 3> ansible_ssh_pass=<Password>
<IP 4> ansible_ssh_pass=<Password>

[appserver:vars]
ansible_user=<Username> # ( ubuntu in our environment )
```


To save the file press `ctrl+O` and to exit press `ctrl+X`


## Test the changes

Go ahead and run the following command:

```bash
ansible webserver -m ping
```

If all succeeds then you are good to go.


## Well done! 👏
