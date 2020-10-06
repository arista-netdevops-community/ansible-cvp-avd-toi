# LAB 12 - AVD EOS CLI CONFIG GEN EXTENSION

## About

Basic playbook to test and understand how to extend AVD EOS CLI CONFIG GEN Role

## Execute lab

__1. Extend EOS CLI Config Generation Role with a New Option in Existing Feature__

```shell
# Go to lab12
$ cd ../lab12-avd-eos-config-gen-extension

# Connect to a switch and test the CLI command options and outputs for NTP authentication
# Example
ntp authentication-key 2 md5 7 0010161510
ntp authentication-key 1 sha1 7 0835495D1D
ntp authentication-key 3 md5 7 051F031C35
ntp trusted-key 2,7-10
ntp authenticate

# Add authentication for the NTP server in the data-model (according to the CLI order)
$ vi ../collections/ansible-avd/ansible_collections/arista/avd/roles/eos_cli_config_gen/README.md
```

```yaml
ntp_server:
  authentication_key:
    < id_1 >:
      hash: < md5 | sha1 >
      encrypted_key: < key >
    < id_2 >:
      hash: < md5 | sha1 >
      encrypted_key: < key >
  trusted_key: < list of key numbers >
  authenticate: < true | false >
  local_interface:
    vrf: < vrf_name >
    interface: < source_interface >
  nodes:
    - < ntp_server_1 >
    - < ntp_server_2 >
```

```shell
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

management telnet
   no shutdown

# Add telnet management in the data-model (according to the CLI order)
$ vi ../collections/ansible-avd/ansible_collections/arista/adv/roles/eos_cli_config_gen/README.md
```

```yaml
management_telnet:
  shutdown: <true | false >
```

```shell
# Create a telnet management jinja template
$  vi ../collections/ansible-avd/ansible_collections/arista/adv/roles/eos_cli_config_gen/templates/eos/management-telnet.j2
```

```jinja
# Management telnet #}
{% if management_telnet is defined and management_telnet is not none %}
!
management telnet
{%     if management_telnet.shutdown is defined and management_telnet.shutdown == false %}
   no shutdown
{%     endif %}
{% endif %}
```

```shell
# Add telnet management in the eos-intended-config.j2
vi ../collections/ansible-avd/ansible_collections/arista/adv/roles/eos_cli_config_gen/eos-intended-config.j2
```

```jinja
{% if management_telnet is defined and management_telnet is not none %}
### Management Telnet

{%     if management_telnet.shutdown is defined and management_telnet.shutdown == false %}
   Management Telnet is enabled

{%     endif %}

### Management Telnet Configuration

{% include 'eos/management-telnet.j2' %}

{% else %}
Management Telnet is not defined.
{% endif %}
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