# TOI Lab preparation using Arista Test Drive.

Connect to ATD Jump box using web interface. For more information ask to your local SE.

## Clone repository

> For people already running virtual env

Run TOI in a python's virtual environment:

```shell
# Clone repository
git clone https://github.com/titom73/ansible-cvp-avd-toi.git

# Move to directory
cd ansible-cvp-avd-toi
```

### Install Requirements

```shell
$ pip install --user -r requirements.txt
```

### Configure CloudVision IP Address

> Inventory is pre-configured with internal CVP IP address, so it is not required to update this file.

Go to [`labs`](../labs/) folder and do the following command:

```shell
$ cd labs

# Edit inventory file
$ vim inventory.yml
```
