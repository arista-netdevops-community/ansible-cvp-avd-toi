# LAB 04 - Manage Configlet on Cloudvision.

## About

Create, Update and Delete configlets on CloudVision.

## Execute lab

__1. Review configlet vars__

```shell
# Go to lab04 folder
$ cd labs/lab04-cv-configlets

# Check variables
$ cat group_vars/CVP.yml

---
CVP_CONFIGLETS:
  01TRAINING-alias: "alias a{{ 999 | random }} show version"
  01TRAINING-01: "alias a{{ 999 | random }} show version"
```

__2. Create 2 new configlets on CloudVision server__

```shell
# Run playbook to create configlets on CVP
$ ansible-playbook playbook.configlet.yml

```

Check on CVP that the configlets have been imported:
https://{{cvp_address}}/cv/provisioning/configlet


__3. Optional: Add a new configlet from file__

Create configlet with content of file [configlet.txt](configlet.txt)

Edit [CVP.yml](group_vars/CVP.yml) file

```
$ vim group_vars/CVP.yml
```

And update content with

```yaml
---
CVP_CONFIGLETS:
  01TRAINING-alias: "alias a{{ 999 | random }} show version"
  01TRAINING-01: "alias a{{ 999 | random }} show version"
  01TRAINING-02: "{{lookup('file', '../configlet.txt')}}"


# Run playbook to create the new configlet on CVP
$ ansible-playbook playbook.configlet.yml
```

Check on CloudVision: https://{{cvp_address}}/cv/provisioning/configlet

__4. Remove configlet 01TRAINING-01 from CloudVision__

Edit [CVP.yml](group_vars/CVP.yml) file

```
$ vi group_vars/CVP.yml
```

And update content with

```yaml
---
CVP_CONFIGLETS:
  01TRAINING-alias: "alias a{{ 999 | random }} show version"
```

Run playbook and check on CloudVision

__5. Remove all configlets created in this lab__

- Edit playbook
- Set mode to absent
- Run playbook
