# LAB 09 - AVD EOS CLI CONFIG GEN

## About

Basic playbook to test and understand AVD EOS CLI CONFIG GEN Role

## Execute lab

__1. Use Ansible to Generate CLI Configuration Files and per Device Documentation__

```shell
# Go to lab09
$ cd ../lab09-avd-eos-cli-config-gen

# Run playbook to generate the structured configuration YAML files and Fabric documentation
$ ansible-playbook playbook.build.structured.yml

# Run playbook to generate the intended EOS configuration files and per device documentation
$ ansible-playbook playbook.build.intended.yml

# Check the playbook and identify the AVD role
$ more playbook.build.intended.yml

# Check the rendered EOS configuration files
$ ls intended/configs
$ more intended/configs/DC1-SPINE1.cfg
$ more intended/configs/DC1-LEAF1A.cfg

# Check the rendered Fabric documentation
$ ls documentation/devices/
$ more documentation/devices/DC1-SPINE1.md
```

__2. Understand the per Device YAML Structured Configuration__

```shell
# Disable LANZ Streaming to CVP for DC1-SPINE1 and DC1-SPINE2 by modifying the per device structured configuration files
# Hint: check first the configuration CLI commands and then check the data-model
# on the following link: https://github.com/aristanetworks/ansible-avd/tree/devel/ansible_collections/arista/avd/roles/eos_cli_config_gen
# Add the following lines in intended/structured_configs/DC1-SPINE1.yml and intended/structured_configs/DC1-SPINE2.yml
queue_monitor_streaming:
  enable: false

# Shutdown Ethernet6 interface on DC1-LEAF1A by modifying the per device structured configuration files
# Add the following lines in intended/structured_configs/DC1-LEAF1A.yml
ethernet_interfaces:
  Ethernet6:
    shutdown: true

# Run playbook to generate the EOS configuration files with the new changes/additions
$ ansible-playbook playbook.build.intended.yml

# Check the newly rendered EOS configuration files and documentation
```