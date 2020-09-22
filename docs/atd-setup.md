# TOI Lab preparation using Arista Test Drive.

Connect to ATD Jump box using web interface. For more information ask to your local SE.

## Clone repository

> For people already running virtual env

Run TOI in a python's virtual environment:

```shell
# Install virtualenv if not part of your system
$ python -m pip install virtualenv

# Activate virtualenv
$ source .venv/bin/activate
```

## Install Python requirements + Ansible

```shell
$ pip install –-requirement requirements.txt

# Check what has been installed
$ pip list

# Install sshpass
$ sudo apt-get install -y sshpass
```

## Install the Arista CVP and AVD collections

```shell
# Create a folder for the Ansible collections
$ mkdir collections
$ cd collections

$ git clone https://github.com/aristanetworks/ansible-cvp.git
$ git clone https://github.com/aristanetworks/ansible-avd.git
```

## Configure CloudVision IP Address

> Inventory is pre-configured with internal CVP IP address, so it is not required to update this file.

Go to [`labs`](../labs/) folder and do the following command:

```shell
$ cd labs

# Edit inventory file
$ vim inventory.yml
```
