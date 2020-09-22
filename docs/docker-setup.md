# TOI Lab preparation using docker image

All the labs have been built using Arista Test Drive topology. Start an ATD or ask to your Arista representative to access to a lab.

To run labs, you can use either a docker container pre-configured or configure your system with Python and virtualenv.

## Use docker image

> Preferred approach

Execute command:

```shell
# Clone repository
git clone https://github.com/arista-netdevops-community/ansible-cvp-toi.git

# Move to directory
cd ansible-cvp-toi

# Start docker container
$ docker run -it --rm -v $(PWD):/project avdteam/base:latest

# (Optional) Shortcut of previous command
$ make run
```

## Configure CloudVision IP Address

Required if you have to run `arista.cvp` ansible collection in [labs](../labs) section.

Go to [`labs`](../labs/) folder and do the following command:

```shell
# Move to lab folder
$ cd labs

# Edit inventory file
$ vim inventory.yml
```
