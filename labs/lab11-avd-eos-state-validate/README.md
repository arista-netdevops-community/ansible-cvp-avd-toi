# LAB 09 - AVD EOS CLI CONFIG GEN

## About

Basic playbook to test and understand AVD EOS STATE VAlIDATIOn Role

## Execute lab

__1. Use Ansible to Deploy Configuration via CVP__

```shell
# Go to lab11
$ cd ../lab11-avd-eos-state-validate

# Run playbook to generate the structured configuration YAML files and the EOS configuration
$ ansible-playbook playbook.build.structured.yml
$ ansible-playbook playbook.build.intended.yml

# Run playbook to deploy the configuration files to CVP
$ ansible-playbook playbook.deploy.CVP.yml

# Run playbook to validate network state after deployment
$ ansible-playbook playbook.validate.state.yml

# Check the playbook and identify the AVD role
$ more playbook. playbook.validate.state.yml

# Check the rendered CVS and Markdown files
```