# LAB 03 - CloudVision Collection overview

## About

Basic commands to test ansible with a basic installation

## Execute lab

__1. Collect facts from CloudVision__

```shell
# Move to lab directory
$ cd ../lab03-arista.cvp-overview

# Display module documentation
$ ansible-doc arista.cvp.cv_device

# Run playbook to collect CVP facts
$ ansible-playbook playbook.facts.yml
```

__3. Optional: Collect only facts for devices__

Get and display facts of active devices with their configuration

```yaml
---
- name: lab03 - cv_facts lab
  hosts: CloudVision
  connection: local
  gather_facts: no
  tasks:
    - name: "Gather CVP facts {{inventory_hostname}}"
      arista.cvp.cv_facts:
        facts:
          devices
        gather_subset:
          config
      register: cv_facts

    - name: "Print out facts from {{inventory_hostname}}"
      debug:
        msg: "{{cv_facts}}"
```
