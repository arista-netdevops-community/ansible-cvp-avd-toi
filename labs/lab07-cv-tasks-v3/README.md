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
  leaf1-bgp: "{{lookup('file','vars/leaf1-bgp.cfg')}}"
  leaf1-mlag: "{{lookup('file','vars/leaf1-mlag.cfg')}}"

CVP_DEVICES:
  - fqdn: 'leaf1'
    parentContainerName: Training
    configlets:
        - minimal
        - leaf1-mlag
        - leaf1-bgp
    imageBundle: []  # Not yet supported
```

__2. Run the playbook to attach the above configlets to leaf1__

```shell
# Run playbook to run task
$ ansible-playbook playbook.tasks.yml
```
