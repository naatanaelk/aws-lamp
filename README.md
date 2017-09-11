# AWS-LAMP: Playbooks to provision a basic LAMP stack on AWS

There are two playbooks you need to run:
- provision-ec2.yml
- install-stack.yml

## Provision EC2
This playbook creates two web server instances, a MySQL instance and an HAProxy instance using the CentOS 6 HVM AMI.

To provision all instances, run:

```
ansible-playbook provision-ec2.yml
```
You will need to install the boto Python module and configure a .boto file in your home directory with your AWS access key and AWS secret key. You can find more info at the [Getting Started with Boto](http://boto.cloudhackers.com/en/latest/getting_started.html) site.

## Install the Stack
This playbook will install and configure all the necessary packages on the Apache, MySQL and HAProxy instances. It will also configure iptables and create a sample index.html file to illustrate the load balancing.

To install the stack, first update the following variables in group_vars/all

- instance_type
- region
- key_pair
- security_group
- volume_size (optional)

Then run the playbook:

```
ansible-playbook -u centos install-stack.yml
```
You will need to need to download the [EC2 dynamic inventory](https://raw.github.com/ansible/ansible/devel/contrib/inventory/ec2.py) script and [INI file](https://raw.githubusercontent.com/ansible/ansible/devel/contrib/inventory/ec2.ini). Copy these to /etc/ansible and then set a variable:

```
export ANSIBLE_HOSTS=/etc/ansible/ec2.py
```

You will also need to disable ssh key checking by running

```
export ANSIBLE_HOST_KEY_CHECKING=False
```

Once the playbook has completed successfully, you can reach the site by going to

```
http://<public_ip_of_load_balancer>:8080
```
Refresh the site several times to see the index.html files of both web servers behind the load balancer.
