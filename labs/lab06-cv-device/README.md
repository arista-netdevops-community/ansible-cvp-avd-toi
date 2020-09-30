# LAB 06 - Manage device on CloudVision

## About

Manage configlets attached to a device

## Execute lab

__1. Review configlet vars__

```shell
# Go to lab06 directory
$ cd ../lab06-cv-device
```

```yaml
$ cat group_vars/CVP.yml

---
CVP_DEVICES:
  leaf1:
    name: 'leaf1'
    parentContainerName: Leaf
    configlets:
        - 'Leaf1-BGP-Lab'
        - 'BaseIPv4_Leaf1'
    imageBundle: []  # Not yet supported
```

__2. Attach configlet `Leaf1-BGP-LAB` to leaf1 device__

```shell
# Run playbook to attach the configlet named Leaf1-BGP-LAB to Leaf1
$ ansible-playbook playbook.device.yml
```

__3. Optional: Create new configlets and attach them to leaf1__

```shell
# Create new configlet to attach to Leaf1
$ vi group_vars/CVP.yml

---
CVP_CONFIGLETS:
  01TRAINING-alias: "alias a{{ 999 | random }} show version"
  01TRAINING-01: "alias a{{ 999 | random }} show version"

CVP_DEVICES:
  leaf1:
    name: 'leaf1'
    parentContainerName: Leaf
    configlets:
        - 'Leaf1-BGP-Lab'
        - 'BaseIPv4_Leaf1'
        - '01TRAINING-01'
    imageBundle: []  # Not yet supported

# Run playbook again to attach the newly created configlet to Leaf1
$ ansible-playbook playbook.device.yml
```