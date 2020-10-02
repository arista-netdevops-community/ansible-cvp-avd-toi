# LAB 12 - AVD EOS CLI CONFIG GEN EXTENSION

## About

Basic playbook to test and understand how to extend AVD EOS CLI CONFIG GEN Role

## Execute lab

__1. Extend EOS CLI Config Generation Role with a New Option in Existing Feature__

```shell
# Go to lab12
$ cd ../lab12-avd-eos-config-gen-extension

# Connect to a switch and test the CLI command options and outputs for NTP authentication

# Add authentication for the NTP server in the data-model (according to the CLI order)
$ vi ../collections/ansible-avd/ansible_collections/arista/adv/roles/eos_cli_config_gen/README.md

# Add authentication for the NTP server in the EOS jinja template
$  vi ../collections/ansible-avd/ansible_collections/arista/adv/roles/eos_cli_config_gen/templates/eos/ntp-servers.j2

# Add authentication for the NTP server in the jinja template for the device documentation
$ vi ../collections/ansible-avd/ansible_collections/arista/adv/roles/eos_cli_config_gen/templates/documentation/ntp-servers.j2

```

__2. Test the New Option in Existing Feature of EOS CLI Config Generation Role__

```shell
# Run the playbook to generate the intended EOS configuration files and per device documentation # again to make sure the new NTP authentication option did not break anything
$ ansible-playbook playbook.build.intended.yml

# Add authentication for the NTP server in spine11
$ vi intended/structured_configs/spine1.yml

# Run the playbook again to generate the new configuration and the documentation
$ ansible-playbook playbook.build.intended.yml

# Verify the rendered configuration
$ more intended/configs/spine1.cfg

# Verify the rendered device documentation
$ more documentation/devices/spine1.md
```

__3. Extend EOS CLI Config Generation Role with a New Feature__

```shell
# Connect to a switch and test the CLI command options and outputs for enabling telnet in
# the default VRF

# Add telnet management in the data-model (according to the CLI order)
$ vi ../collections/ansible-avd/ansible_collections/arista/adv/roles/eos_cli_config_gen/README.md

# Create a telnet management jinja template
$  vi ../collections/ansible-avd/ansible_collections/arista/adv/roles/eos_cli_config_gen/templates/eos/management-telnet.j2

# Add telnet management in the eos-intended-config.j2
vi ../collections/ansible-avd/ansible_collections/arista/adv/roles/eos_cli_config_gen/eos-intended-config.j2
```

__4. Test New Feature of EOS CLI Config Generation Role__

```shell
# Run the playbook to generate the intended EOS configuration files and per device documentation # again to make sure the new telnet feature did not break anything
$ ansible-playbook playbook.build.intended.yml

# Enable telnet management in the default VRF for spine1
$ vi intended/structured_configs/spine1.yml

# Run the playbook again to generate the new configuration
$ ansible-playbook playbook.build.intended.yml

# Verify the rendered configuration
$ more intended/configs/spine1.cfg

# Create a telnet management jinja template for the device documentation
$ vi ../collections/ansible-avd/ansible_collections/arista/adv/roles/eos_cli_config_gen/templates/documentation/management-telnet.j2

# Add telnet management in the eos-device-documentation.j2
vi ../collections/ansible-avd/ansible_collections/arista/adv/roles/eos_cli_config_gen/eos-intended-config.j2

# Run the playbook again to generate the new device documentation
$ ansible-playbook playbook.build.intended.yml

# Verify the rendered device documentation
$ more documentation/devices/spine1.md

```