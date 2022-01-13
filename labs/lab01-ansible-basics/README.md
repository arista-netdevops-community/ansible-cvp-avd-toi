# LAB 01 - Ansible Basics

## About

Basic commands to test ansible with a basic installation

## Execute lab

__1. Identify Ansible main components__

```shell
# List the files and folders used in this Ansible setup
$ cd labs/lab01-ansible-basics
$ ls

# View the inventory
$ more inventory.yml

# View the ansible configuration definition and look at the inventory being used and the path to the collections
$ more ansible.cfg

# View the installed collections
$ ls ../collections

# View all the ansible command options
$ ansible --help
```

__2. Test Ansible installation__

```shell
$ ansible 127.0.0.1 -m ping

127.0.0.1 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

__3. Display variables for a given host or a given group__

```shell
# List all the variables stored in the different ansible files for the DC_SPINES group
$ ansible DC_SPINES -m debug -a "var=hostvars[inventory_hostname]"

# List all the variables stored in the different ansible files for the Leaf1 host
$ ansible Leaf1 -m debug -a "var=hostvars[inventory_hostname]"

# From where does Leaf1 inherit the variables below:
 "ntp_servers": [
            "192.168.0.2",
            "192.168.0.3"
        ],
# Check in group_vars

# Why does it inherit these variables?
# Check the inventory file and see the groups leaf1 belongs to. This is also visible at the beginning of the output of the following command: ansible Leaf1 -m debug -a "var=hostvars[inventory_hostname]"

# Another command to list variables
$ ansible-inventory --host Leaf1
```

> Update `inventory.yml` with the generated password of your ATD instance

__4. Run Ad-hoc commands and analyse the differences__

```shell
# Run show version on Spine1
$ ansible Spine1 -m eos_command -a "commands='show version | grep image'"

# Run show version on Spine1 with JSON output
$ ansible Spine1 -m eos_command -a "commands='show version | json'"
# Run show run on all the switches of the DC
$ ansible DC -m eos_command -a "commands='show runn'"

# Run show run on Spine1 only by using the command option --limit
$ ansible DC --limit Spine1 -m eos_command -a "commands='show runn'"

# Run show version on the Spine switches using another inventory, in this case using inventory_cli.yml
# Note that inventory_cli uses a different connection method to the switches : CLI over SSH
$ ansible DC_SPINES -i inventory_cli.yml -m eos_command -a "commands='show version | grep image'"

# Run show version on the Spine switches using Test_inventory.yml and the verbose mode
$ ansible DC -i inventory_cli.yml -m eos_command -a "commands='show version | grep image'" -vvv
```

__5. Use playbooks to collect EOS version and to configure__

```shell
# Run playbook to collect eos version
$ ansible-playbook playbook.eos_version.yml

# Run playbook to collect eos version of the Spine switches only using the --limit option
$ ansible-playbook --limit DC_SPINES playbook.eos_version.yml

# Run playbook to collect eos version of the Spine switches only from the test inventory
$ ansible-playbook --limit DC_SPINES -i Test_inventory.yml playbook.eos_version.yml

# Run playbook to collect eos version of the Spine switches only from the test inventory in verbose mode
$ ansible-playbook --limit DC_SPINES -i Test_inventory.yml playbook.eos_version.yml -vvv

# Run playbook to configure a description on Ethernet interface 1 of Leaf1
$ ansible-playbook playbook.ethernet_descr.yml

# Use an ansible raw command to verify that the configuration has been applied
$ ansible Leaf1 -m eos_command -a "show running interface Ethernet1"
```

__6. Create simple playbooks__

```shell
# Create a simple playbook to collect LLDP neighbors
$ cp playbook.eos_version.yml playbook.lldp_nei.yml
# Then change the tasks name, the commands, the variable and the message to the required information

# Create a playbook to configure a new vlan 100 named TEST
$ cp playbook.ethernet_descr.yml playbook.vlan.yml
# Then change the tasks name and the commands to the required information
$ more playbook.vlan.yml
---
- name: Configure vlan 100
  hosts: DC_LEAFS
  gather_facts: false
  tasks:
    - name: Configure vlan 100 on Leaf switches
      eos_config:
         lines:
            - name TEST
         parents: vlan 100
```