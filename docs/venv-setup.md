# TOI Lab preparation using Python Virtual Environment

All the labs have been built using Arista Test Drive topology. Start an ATD or ask to your Arista representative to access to a lab.

To run labs, you can use either a docker container pre-configured or configure your system with Python and virtualenv.

## Install Virtual Environment

> For people already running virtual env

Run TOI in a python's virtual environment:

```shell
# Clone repository
git clone https://github.com/titom73/ansible-cvp-toi.git

# Move to directory
cd ansible-cvp-avd-toi
```

### Install Python Virtual Environment

> Other venv setup might be used as well.

```shell
# Upgrade pip if needed
/usr/local/bin/python -m pip install --upgrade pip

# Install virtualenv if not part of your system
$ python -m pip install virtualenv

# Add the path
$ export PATH=/usr/local/bin:/usr/bin:/bin:/usr/games:/home/arista/.local/bin

# Verify path
$ echo $PATH

# Create a virtual env named .venv
$ virtualenv --no-site-packages -p $(which python3) .venv

# Activate virtualenv
$ source .venv/bin/activate
```

### Install Requirements

```shell
$ pip install -r requirements.txt

# Check what has been installed
$ pip list

# Create a folder for the Ansible collections
$ mkdir collections
$ cd collections

# Install the Arista CVP and AVD collections
$ ansible-galaxy collection install arista.cvp
$ ansible-galaxy collection install arista.avd
$ cd ..
```

### Configure CloudVision IP Address

Go to [`labs`](../labs/) folder and do the following command:

```shell
$ cd labs

# Edit inventory file
$ vim inventory.yml
```
