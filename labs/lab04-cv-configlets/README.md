# LAB 04 - Manage Configlet on Cloudvision

## About

Create, Update and Delete configlets on CloudVision

## Execute lab

__1. Review configlet vars__

```shell
# Go to lab04 directory
$ cd ../lab04-cv-configlets

# Check variables
$ cat group_vars/CVP.yml
```

```yaml
---
CVP_CONFIGLETS:
  01TRAINING-alias: "alias a{{ 999 | random }} show version"
  01TRAINING-01: "alias a{{ 999 | random }} show version"
```

__2. Create a new configlet on CloudVision server__

Create a new configlet

```shell
# Create a new configlet by editing group_vars/CVP.yml
$ vi group_vars/CVP.yml
```

```yaml
---
CVP_CONFIGLETS:
  01TRAINING-alias: "alias a{{ 999 | random }} show version"
  01TRAINING-01: "alias a{{ 999 | random }} show version"
  01TRAINING-02: "alias a{{ 999 | random }} show version"
```

```shell
# Run playbook to create configlets on CVP
$ ansible-playbook playbook.configlet.yml
```

Check on CVP that the configlet has been imported:
https://{{cvp_address}}/cv/provisioning/configlet

__3. Optional: Add a new configlet from file__

Create configlet with content of file [configlet.txt](configlet.txt)

```shell
$ Edit [CVP.yml](group_vars/CVP.yml) file
$ vi group_vars/CVP.yml

# And update content with
```

```yaml
---
CVP_CONFIGLETS:
  01TRAINING-alias: "alias a{{ 999 | random }} show version"
  01TRAINING-01: "alias a{{ 999 | random }} show version"
  01TRAINING-02: "alias sib show interface brief"
  01TRAINING-03: "{{lookup('file', './configlet.txt')}}"
```

```shell
# Run playbook to create the new configlet on CVP
$ ansible-playbook playbook.configlet.yml
```

Check on CloudVision: https://{{cvp_address}}/cv/provisioning/configlet

__4. Remove configlet 01TRAINING-01 to -03 from CloudVision__

Remove configlets 01TRAINING-01, 01TRAINING-02 and 01TRAINING-03

```shell
$ Edit [CVP.yml](group_vars/CVP.yml) file
$ vi group_vars/CVP.yml

# And update content with
```

```yaml
---
CVP_CONFIGLETS:
  01TRAINING-alias: "alias a{{ 999 | random }} show version"
```

```shell
# Run playbook to create the new configlet on CVP
$ ansible-playbook playbook.configlet.yml
```

Check on CloudVision: https://{{cvp_address}}/cv/provisioning/configlet

__5. Remove all configlets created in this lab__

- Edit playbook
- Set state to absent
- Run playbook
