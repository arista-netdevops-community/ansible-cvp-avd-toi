# LAB 05 - Manage Containers on CloudVision

## About

Create and update containers on CloudVision

## Execute lab

__1. Review containers vars__

```shell
# Go to lab05 directory
$ cd ../lab05-cv-containers-v3
```

```yaml
$ cat group_vars/CVP.yml

---
CVP_CONFIGLETS:
  01TRAINING-alias: "alias srnz show interfaces counters rates | nz"
  01TRAINING-01: "alias a{{ 999 | random }} show version"

CVP_CONTAINERS:
  TRAINING:
    parentContainerName: Tenant
    configlets:
    - 01TRAINING-01
  TRAINING_DC:
    parentContainerName: TRAINING
  TRAINING_LEAFS:
    parentContainerName: TRAINING_DC
  TRAINING_SPINES:
    parentContainerName: TRAINING_DC
DEVICES:
  - fqdn: Spine1
    parentContainerName: TRAINING_SPINES
    configlets:
    - 01TRAINING-alias
```

__2. Create containers and move the device__

```shell
# Run playbook to create containers and move device
$ ansible-playbook create_topo.yml
```

> On cloudvision server, cancel task and move back the device to its initial container

__3. Optional: Attach 01TRAINING-01 to TRAINING container__

Edit [CVP.yml](group_vars/CVP.yml) file

```shell
# Attach 01TRAINING-01 to TRAINING container and remove Spine1 under the container TRAINING_SPINES
$ vi group_vars/CVP.yml
```

And update content with the following:

```yaml
---
CVP_CONFIGLETS:
  01TRAINING-alias: "alias srnz show interfaces counters rates | nz"
  01TRAINING-01: "alias a{{ 999 | random }} show version"

CVP_CONTAINERS:
  TRAINING:
    parentContainerName: Tenant
    configlets:
    - 01TRAINING-alias
    - 01TRAINING-01
  TRAINING_DC:
    parentContainerName: TRAINING
  TRAINING_LEAFS:
    parentContainerName: TRAINING_DC
  TRAINING_SPINES:
    parentContainerName: TRAINING_DC
```

```shell
# Run playbook to attach the configlet to the container
$ ansible-playbook playbook.container.v3.yml
```

It will fail. Why does it fail? How can you solve the issue?
Hint: is the configlet that you are trying to attach to the container present on CloudVision?

__4. Remove Configlet from a Device__

Add [remove_configlet.yml](remove_configlet.yml) file
```shell
# Remove 01TRAINING-01 from Spine1
$ vi remove_configlet.yml
```

```yaml
---
- name: lab05 - cv_container_v3 lab
  hosts: CloudVision
  connection: local
  gather_facts: no
  tasks:
    - name: "Remove configlets from devices on {{inventory_hostname}}"
      arista.cvp.cv_device_v3:
        devices: "{{DEVICES}}"
        search_key: 'fqdn'
        apply_mode: strict
        state: present
```

Edit [CVP.yml](group_vars/CVP.yml) file

```yaml
---
CVP_CONFIGLETS:
  01TRAINING-alias: "alias a{{ 999 | random }} show version"
  01TRAINING-01: "alias a{{ 999 | random }} show version"

CVP_CONTAINERS:
  TRAINING:
    parentContainerName: Tenant
    configlets:
    - 01TRAINING-01
  TRAINING_DC:
    parentContainerName: TRAINING
  TRAINING_LEAFS:
    parentContainerName: TRAINING_DC
  TRAINING_SPINES:
    parentContainerName: TRAINING_DC
DEVICES:
  - fqdn: Spine1
    parentContainerName: TRAINING_SPINES
```

```shell
# Run playbook
$ ansible-playbook remove_configlet.yml
```
Then check on CloudVision.

> NOTE: The above action will remove all configlets from `Spine1`, even the manually configured ones. If you need to retain those, add them under `DEVICES` in `CVP.yml`, like so

```yaml
DEVICES:
  - fqdn: Spine1
    parentContainerName: TRAINING_SPINES
    configlets:
      - MANUALLY-ADDED-CONFIGLET1
```

__5. Remove Device from a container__

Add [move_device_from_configlet.yml](move_device_from_configlet.yml) file

```shell
# Add move_device_from_configlet.yml
$ vi move_device_from_configlet.yml
```
```yaml
---
- name: lab05 - cv_container_v3 lab
  hosts: CloudVision
  connection: local
  gather_facts: no
  tasks:
    - name: "Configure devices on {{inventory_hostname}}"
      arista.cvp.cv_device_v3:
        devices: "{{DEVICES}}"
        search_key: 'fqdn'
        apply_mode: loose
        state: present
      register: CVP_CONTAINERS_RESULT
```

```shell
# Edit CVP.yml and change the container for Spine1
# vi CVP.yml
```

```yaml
---
CVP_CONFIGLETS:
  01TRAINING-alias: "alias a{{ 999 | random }} show version"
  01TRAINING-01: "alias a{{ 999 | random }} show version"

CVP_CONTAINERS:
  TRAINING:
    parentContainerName: Tenant
    configlets:
    - 01TRAINING-01
  TRAINING_DC:
    parentContainerName: TRAINING
  TRAINING_LEAFS:
    parentContainerName: TRAINING_DC
  TRAINING_SPINES:
    parentContainerName: TRAINING_DC
DEVICES:
  - fqdn: Spine1
    parentContainerName: Spine
```

```shell
# Run playbook to move Spine1 to Spine container
$ ansible-playbook move_device_from_configlet.yml
```

Check on CloudVision to see which container Spine1 is under.

__6. Remove Container topology__

```shell
# Edit playbook and change mode to `delete`
$ vi remove_topo.yml
```

```yaml
- name: "Configure containers on {{inventory_hostname}}"
  arista.cvp.cv_container_v3:
    topology: "{{CVP_CONTAINERS}}"
    state: absent
    register: CVP_CONTAINERS_RESULT
```

```shell
# Run playbook to delete containers
$ ansible-playbook remove_topo.yml
```

Then check on CloudVision.
