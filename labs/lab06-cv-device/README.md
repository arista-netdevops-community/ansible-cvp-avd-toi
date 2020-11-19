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
# Check variables before running playbook
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
# Run playbook to attach the configlet to Leaf1
$ ansible-playbook playbook.device.yml
```

Check on CloudVision: https://{{cvp_address}}/cv/provisioning

__3. Optional: Create new configlets and attach them to leaf1__

```shell
# Create new configlet to attach to Leaf1
$ vi group_vars/CVP.yml
```

```yaml
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
```

```shell
# If 01TRAINING-01 configlet is not present on CVP, the playbook will fail with the following message:
"msg": "leaf1 device has unknown configlets from CV: ['01TRAINING-01']"
# If not present, modify the playbook to push the configlet before applying it to the device. Do not forget to
# collect CVP facts.
# Be careful with the YAML indentation
$ vi ansible-playbook playbook.device.yml

# Run playbook again to attach the newly created configlet to Leaf1
$ ansible-playbook playbook.device.yml
```

Check on CloudVision: https://{{cvp_address}}/cv/provisioning