# LAB 11 - AVD EOS STATE VALIDATION

## About

Basic playbook to test and understand AVD EOS STATE VAlIDATION Role

## Execute lab

__1. Use Ansible to Validate the Network State__

!! Note that completing lab10 is a prerequisite to start this lab !!

```shell
# Go to lab11
$ cd ../lab11-avd-eos-state-validate

# Check the playbook and identify the AVD role
$ more playbook. playbook.validate.state.yml

# Run playbook to validate network state after the deployment made in lab10
$ ansible-playbook playbook.validate.state.yml

# Check the rendered Markdown file
$ more validation/DC1_FABRIC-state.md

# Check only the failed tests in the rendered Markdown file
$ more validation/DC1_FABRIC-state.md | grep FAIL
```

__2. Find the Issues, Fix and Use Ansible to Validate the Network State Again__

```shell
# Run playbook to only check ip reachability
$ ansible-playbook playbook.validate.state.yml --tags ip_reachability

# More info about the different tags:
# https://github.com/aristanetworks/ansible-avd/tree/devel/ansible_collections/arista/avd/roles/eos_validate_state

# Check the rendered Markdown file
$ more validation/DC1_FABRIC-state.md

# Check only the failed tests in the rendered Markdown file
$ more validation/DC1_FABRIC-state.md | grep FAIL

# Run playbook to only check spine1
$ ansible-playbook playbook.validate.state.yml –limit spine1

# Check the rendered Markdown file
$ more validation/DC1_FABRIC-state.md

# Connect to spine1 and enable interface ethernet2

# Run playbook to validate network state after the change
$ ansible-playbook playbook.validate.state.yml

# Check the rendered Markdown file
$ more validation/DC1_FABRIC-state.md
$ more validation/DC1_FABRIC-state.md | grep FAIL
```