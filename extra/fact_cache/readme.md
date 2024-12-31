# Overview

Cache plugins allow Ansible to store gathered facts or saved facts data without the performance hit of retrieving them from the source.

By default, the facts are cached in memory and gathered every time you run Playbook. On this documentation, we will discuss how to cache facts on `redis`



## Duration

30 minutes

## Requirements

* Redis
  * Docker
  * AWS Memory
  * Native Install
* Python `redis` plugin



_**Note:**_ 
* We use Docker for simplicity.
* Docker is already **_installed_** on your lab machine.
* You can follow the steps in the [Installing Docker](https://github.com/farshidmh/docker-labs/tree/main/install) guide to install Docker on your machine. 


## Create redis container

To setup a redis container, run the following command:

```bash
docker run -d -p 6379:6379 redis
```

output will be like:

```bash 
0sdfg0dfg0ert067r0tyhs0f0e4r30w534506546075763450563455
```
**Note:** 
* Yours might be different.
* This step might take upto 5 minutes to complete.

## Install python redis plugin

To install the python redis plugin, run the following command:

```bash
pip install redis
```

output:

```bash
Installing collected packages: redis
Successfully installed redis-4.3.4
```
**Note:** your installed version might differ.


## Ansible Config file

To enable this plugin, we need to add the redis info to the `defaults` section of the `ansible.cfg` file.

* Open config file with sudo access

```
sudo nano /etc/ansible/ansible.cfg
```

* Add the following line to the `defaults` section:

```
gathering = smart
fact_caching = redis
fact_caching_timeout = 3600
fact_caching_connection = localhost:6379:0
fact_caching_prefix = ansible_facts_
```

`gathering` setting controls the default policy of facts gathering

* The value ‘implicit’ is the default, which means that the fact cache will be ignored and facts will be gathered per play unless ‘gather_facts: False’ is set. 


* The value ‘explicit’ is the inverse, facts will not be gathered unless directly requested in the play. 


* The value ‘smart’ means each new host that has no facts discovered will be scanned, but if the same host is addressed in multiple plays it will not be contacted again in the playbook run. This option can be useful save fact gathering time. Both ‘smart’ and ‘explicit’ will use the fact cache.

`fact_caching_timeout` This option tells Ansible when to expire values from the cache. Setting this value to 0 effectively disables expiry, and a positive value is a TTL in seconds.


After enabling this plugin, you will notice that your playbooks will run faster.


## Well done! 👏