<link rel='stylesheet' href='../assets/css/main.css'/>

# Overview

In this lab, You will learn how to install, update and remove packages on your hosts using `apt` module.

## Duration

25 minutes

## Step 1 - getting to know the Module `apt`

`apt` module is used Manages packages on `Debian/Ubuntu`, parameters are:

- `name`  the name of the package that you want to install
- `state`:
  - `absent` to delete the packages
  - `latest` to install the latest version of package
  - `present` if it is present, do not update it.

default template looks like:

```bash
ansible <Pattern> -m apt -a "name=<Package> state=<state>"
```

## Step 2 - Install Apache

run the following command to install apache on your hosts, we'll use `appserver`

```bash
ansible <Pattern> -m apt -a "name=apache2 state=present" -b
```

sample:

```bash
ansible webserver -m apt -a "name=apache2 state=present" -b
```

Output will be long, look out for `CHANGED`

Note the `-b` switch, what is that?

Basically we are telling `ansible` to run this command as `root` on all hosts.

Login to each host and verify that `apache2` is installed

Command to verify:

```bash
apache2 -v
```

output

```console
Server version: Apache/2.4.41 (Ubuntu)
Server built:   2021-07-05T07:16:56
```

_Note:_ Dates may vary

## Step 2 — remove the package

To remove the installed package on your hosts:

```bash
ansible <Pattern> -m apt -a "name=apache2 state=absent autoremove=yes" -b
```

What is `autoremove`?

If yes, remove unused dependency packages for all module states except.


## Well done! 👏