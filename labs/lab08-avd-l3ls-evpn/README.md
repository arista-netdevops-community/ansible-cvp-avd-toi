# LAB 08 - AVD L3LS EVPN

## About

Basic playbook to test and understand AVD L3LS EVPN Role.

## Execute lab

__1. Use Ansible to Generate YAML Structured Configuration and Fabric Documentation__

```shell
# Go to lab08
$ $ cd ../lab08-avd-l3ls-evpn

# Run playbook to generate the structured configuration (YAML files) and the fabric documentation
$ ansible-playbook playbook.build.structured.yml

# Check the playbook and identify the AVD
$ more playbook.build.structured.yml

# Check the rendered structured configuration
$ ls intended/structured_configs
$ more intended/structured_configs/DC1-SPINE1.yml
$ more intended/structured_configs/DC1-LEAF1A.yml

# Check the rendered Fabric documentation
$ ls documentation/DC1_FABRIC/
$ more documentation/DC1_FABRIC/ DC1_FABRIC.md

```

__2. Understand the Fabric Description Structure__

```shell
# Modify the IP range used for the VTEP VXLAN Tunnel source loopbacks
# Change vtep_loopback_network_summary value in group_vars/DC1_FABRIC.yml

# Add server03
#	Part of TENANT_B
#	In RackB
#	Connected on port Ethernet 8 of switch DC1-L2LEAF1A
# Add the following lines in group_vars/DC1_SERVERS.yml
  server03:
    rack: RackB
    adapters:
      - type: nic
        server_ports: [Eth0]
        switch_ports: [Ethernet8]
        switches: [DC1-L2LEAF1A]
        profile: TENANT_B

# Run playbook to generate the structured configuration files with the new changes/additions
$ ansible-playbook playbook.build.structured.yml

# Check the newly rendered structured configuration files and documentation
```