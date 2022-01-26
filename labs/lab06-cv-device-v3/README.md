# LAB 06 - Manage device on CloudVision

## About

Manage configlets attached to a device

## Execute lab

__1. Review configlet vars__

```shell
# Go to lab06 directory
$ cd ../lab06-cv-device-v3
```

```yaml
# Check variables before running playbook
$ cat group_vars/CVP.yml

---
CVP_CONFIGLETS:
  leaf1-bgp: "{{lookup('file','vars/leaf1-bgp.cfg')}}"
  leaf1-mlag: "{{lookup('file','vars/leaf1-mlag.cfg')}}"

CVP_CONTAINERS:
  TRAINING:
    parentContainerName: Tenant
  TRAINING_DC:
    parentContainerName: TRAINING
  TRAINING_LEAFS:
    parentContainerName: TRAINING_DC
  TRAINING_SPINES:
    parentContainerName: TRAINING_DC

CVP_DEVICES:
  - fqdn: 'leaf1'
    parentContainerName: TRAINING_LEAFS
    configlets:
        - leaf1-mlag
        - leaf1-bgp
    imageBundle: []  # Not yet supported
```

__2. Attach configlet `leaf1-bgp` and `leaf1-mlag` to leaf1 device__

```shell
# Run playbook to attach the configlet to Leaf1
$ ansible-playbook playbook.device.yml
```

Check on CloudVision: https://{{cvp_address}}/cv/provisioning
