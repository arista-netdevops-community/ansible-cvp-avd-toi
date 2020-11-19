# LAB 10 - AVD EOS CONFIG DEPLOY

## About

Basic playbook to test and understand AVD EOS CONFIG DEPLOY Roles via CVP and eAPI

## Execute lab

__1. Use Ansible to Deploy Configuration via CVP__

```shell
# Go to lab10
$ cd ../lab10-avd-eos-config-deploy

# Run playbook to generate the required folders, the structured YAML configuration files, the Fabric documentation, the intended EOS configuration files and the per device documentation
$ ansible-playbook playbook.build.folders.structured.intended.yml

# Verify that the structured and intended configuration files have been rendered as expected
$ ls intended/structured_configs/
$ ls intended/configs/

# Connect to CVP and check the current topology and device configuration
# https://{{cvp_address}}/cv/provisioning

# Approve and execute the pending tasks in CVP

# Run playbook to deploy the configuration via CVP and to build the container topology
$ ansible-playbook playbook.deploy.CVP.yml

# Connect to CVP and check the new topology and device configuration
# https://{{cvp_address}}/cv/provisioning

# Approve and execute the pending tasks in CVP

# Note that the switches will appear with an inactive streaming status. This is normal. It will takes a few minutes to normalize.

# Check the configuration of spine1 for example
$ ssh arista@192.168.0.10

```

__2. Use Ansible to Deploy Configuration via eAPI__

```shell
# Shutdown interface ethernet2 in the configuration file of spine1
$ vi intended/configs/spine1.cfg

interface Ethernet2
   shutdown

# Run playbook to deploy the configuration via eAPI
$ ansible-playbook playbook.deploy.eAPI.yml

# Check the pre- and post-configuration backup files
$ ls config_backup
$ more config_backup/spine1_pre_running-config.conf
$ more config_backup/spine1_post_running-config.conf

# Check the configuration of spine1
$ ssh arista@192.168.0.10
```