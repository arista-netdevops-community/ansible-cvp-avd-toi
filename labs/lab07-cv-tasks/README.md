# LAB 07 - Manage tasks generated on CloudVision

## About

Execute pending tasks on CloudVision

## Execute lab

__1. Review configlet vars__

```shell
# Go to lab07 directory
$ cd ../lab07-cv-tasks
```

```shell
# Check variables
$ cat group_vars/CVP.yml
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
        - '01TRAINING-alias'
    imageBundle: []  # Not yet supported
```

__2. Attach configlet `Leaf1-BGP-LAB` to leaf1 device__

```shell
# Run playbook to run task
$ ansible-playbook playbook.tasks.yml
```
