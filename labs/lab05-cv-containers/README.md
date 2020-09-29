# LAB 05 - Manage Containers on CloudVision

## About

Create and update containers on CloudVision

## Execute lab

__1. Review containers vars__

```yaml
$ cat group_vars/CVP.yml

---
CVP_CONTAINERS:
  TRAINING:
    parent_container: Tenant
  TRAINING_DC:
    parent_container: TRAINING
  TRAINING_LEAFS:
    parent_container: TRAINING_DC
  TRAINING_SPINES:
    parent_container: TRAINING_DC
    devices:
      - 'spine1'
```

__2. Create containers and move device__

```shell
# Run playbook to create containers and move device
$ ansible-playbook playbook.configlet.yml
```

> On cloudvision server, cancel task and move back device to its initial container

__3. Optional: Attach 01TRAINING-01 to TRAINING container__

Edit [CVP.yml](group_vars/CVP.yml) file

```
$ vi group_vars/CVP.yml
```

And update content with

```yaml
---
CVP_CONFIGLETS:
  01TRAINING-alias: "alias a{{ 999 | random }} show version"
  01TRAINING-01: "alias a{{ 999 | random }} show version"

CVP_CONTAINERS:
  TRAINING:
    parent_container: Tenant
    configlets:
      - '01TRAINING-01'
  TRAINING_DC:
    parent_container: TRAINING
  TRAINING_LEAFS:
    parent_container: TRAINING_DC
  TRAINING_SPINES:
    parent_container: TRAINING_DC
    devices:
      - 'spine1'
```

```
# Run playbook to attach the configlet to the container
$ ansible-playbook playbook.container.yml
```

Then check on CloudVision

> On cloudvision server, cancel task and move back device to its initial container

__4. Remove Container topology__

```
# Edit playbook and change mode to `delete`
$ vi playbook.container.yml
```

```yaml
- name: "Configure containers on {{inventory_hostname}}"
  arista.cvp.cv_container:
    cvp_facts: "{{CVP_FACTS.ansible_facts}}"
    topology: "{{CVP_CONTAINERS}}"
    mode: delete
    register: CVP_CONTAINERS_RESULT
```

```
# Run playbook delete containers
$ ansible-playbook playbook.container.yml
```

Then check on CloudVision