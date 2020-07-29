# ELK Deployment

This is an ansible repository that contains all the documentation and scripts used during the ELK lab.

## Installation:

### Setting up the master

Install [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html?extIdCarryOver=true&sc_cid=701f2000001OH7YAAW#installing-ansible-on-debian):

Clone this repository.

In your computer, on this repository's folder, create an `inventory` file containing the IP addresses of the devices you'd like to set as master nodes. Example:

    51.89.229.198

Modify the `kibana.yml` and `elasticsearch.yml` to specify the host's IP address

Execute the ansible script: &nbsp; `ansible-playbook -i inventory master.yml`

This will provide a VM containing both elasticsearch and kibana. As well as our **webhook** server for deploying Elastic to other machines, or for monitoring them.

### Setting up the slave

On the master VM, there will be two heat templates, one for another master VM called **master_heat.yml**. Another for slave VMs (A VM to be monitored). It's called **slave_heat.yml**

In order to setup a slave VM, create a heat stack using **slave_heat.yml**, using `heat create -t slave_heat.yml slave`

## How it works

Upon creating a heat stack, the created VM will install python and curl, and then send an HTTP request to our webhook server (In order to give it its IP).
After that's done, the server will execute the corresponding Ansible script (master/slave) found in `/var/www/webhook/` as `master.yml` or `slave.yml`

### The master host

This host has its configuration defined in `master.yml` found in `/var/www/webhook`.

#### Configuration steps:

- Install Ansible
- Install ElasticSearch and Kibana
- Copy configuration files
- Enable and start daemons (Elasticsearch and Kibana)
- Download and install Node.js
- Copy webhook's `webhook.js` and `webhook.service` files
- Enable and start webhook daemon
- Copy heat and Ansible scripts for master and slave to `/var/www/webhook`

### The slave host

This host has its configuration defined in `slave.yml` found in `/var/www/webhook`.

#### Configuration steps:

- Download and install metricbeat
- Copy configuration file (Contains Kibana and Elasticsearch credentials)
- Enable system module and setup

## Customizing:

For customized use of this repository, you can change multiple things:

- Kibana and Elasticsearch credentials: Those are the credentials for connecting slave VMs to Elasticsearch, those can be configured in `files/metricbeat.yml`.
