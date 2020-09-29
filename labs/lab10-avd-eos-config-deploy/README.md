# LAB 09 - AVD EOS CLI CONFIG GEN

## About

Basic playbook to test and understand AVD EOS CONFIG DEPLOY Roles via CVP and eAPI

## Execute lab

__1. Use Ansible to Deploy Configuration via CVP__

```shell
# Go to lab10
$ cd ../lab10-avd-eos-config-deploy

# Connect to CVP and check the current topology and device configuration

# Run playbook to deploy the configuration via CVP and build the container topology
$ ansible-playbook playbook.deploy.CVP.yml

# Connect to CVP and the new topology and device configuration
```

__2. Use Ansible to Deploy Configuration via eAPI__

```shell
# Change the configuration of DC1_SPINE1
$ vi intended/configs/DC1_SPINE1.cfg

# Run playbook to deploy the configuration via eAPI
$ ansible-playbook playbook.deploy.eAPI.yml

# Check the pre- and post-configuration backup files
$ ls config_backup
$ more config_backup/DC1_SPINE1_pre_running-config.conf
$ more config_backup/DC1_SPINE1_post_running-config.conf

# Check the configuration of DC1_SPINE1
$ ssh arista@192.168.0.10
```