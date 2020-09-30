# LAB 07 - Manage tasks generated on CloudVision

## About

Execute pending tasks on CloudVision

## Execute lab

__1. Review configlet vars__

```yaml
# Check variables
$ cat group_vars/CVP.yml

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
        - '01TRAINING-01'
    imageBundle: []  # Not yet supported
```

__2. Attach configlet `Leaf1-BGP-LAB` to leaf1 device__

```shell
# Run playbook to run task
$ ansible-playbook playbook.tasks.yml
```
