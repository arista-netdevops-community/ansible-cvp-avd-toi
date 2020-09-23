# LAB 02 - Ansible Template Management

## About

Basic commands to test ansible template management.

## Execute lab

__1. Use Ansible to generate configuration thanks to a jinja2 template__

```shell
# Go to lab02
$ cd ../lab02-ansible-template-mgt

# Run playbook to generate the configuration files
$ ansible-playbook playbook.config_gen.yml

# Check the playbook to see where the configuration files have been rendered
$ more playbook.config_gen.yml

# Check the rendered configuration
$ ls configuration
$ more configuration/leaf1.txt
```

__2. Extend the jinja2 template to generate the configuration of the DNS servers__

```shell
# Add the DNS feature in the config.j2 template
$ vi templates/config.j2
# Add the following lines in the existing templates
{% if name_servers is defined and name_servers is not none %}
!
{%   for node in name_servers %}
ip name-server {{ node }}
{%   endfor %}
{% endif %}

# Run playbook to generate the configuration files with the new added DNS feature
$ ansible-playbook playbook.config_gen.yml

# Check the newly rendered configuration
$ ls configuration
$ more configuration/Leaf1.txt

# Check configuration/Leaf3.txt
$ more configuration/Leaf3.txt

# Why are the name servers IP addresses different?
# Hint: check host_vars/Leaf3.yml
```