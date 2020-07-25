# ELK Deployment

This is an ansible repository that contains all the documentation and scripts used during the ELK lab.

### Installation:

Install [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html?extIdCarryOver=true&sc_cid=701f2000001OH7YAAW#installing-ansible-on-debian):

Clone this repository.

In your computer, on this repository's folder, create an `inventory` file containing the IP addresses of the devices you'd like to set as master nodes. Example:

    51.89.229.198

Execute the ansible script: &nbsp; `ansible-playbook -i inventory master.yml`

This will a VM containing both elasticsearch and kibana. As well as our **webhook** server for deploying Elastic to other machines, or for monitoring them.
