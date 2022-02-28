# My infrastructure

## Installation
1. install python3 and python3-pip
2. install pipenv `python3 -m pip install pipenv`
3. clone repo `git clone git@github.com:piatrou/infrastructure.git`
4. go to repo folder `cd infrastructure`
5. Prepare ansible `pipenv run prepare`

## Preparing

### Create inventory folder 
```commandline
mkdir -p inventories/default
```
### create inventory file

```commandline
touch inventories/default/inventory.yml
```

inventory file sample:
```yaml
all:
  children:
    vault:
      children:
        vpn-servers:
          hosts:

            some-vpn:
              ansible_host: 127.0.0.1
              ansible_user: ansible
              ansible_port: 2222
              ansible_password: 

```

### create vars files 
For group of hosts:
```commandline
mkdir inventories/default/group_vars/
touch inventories/default/group_vars/all.yml
```
Or only for one host
```commandline
mkdir -p inventories/default/host_vars
touch inventories/default/host_vars/some-vpn.yml
```

vars file sample:
```yaml
COMMON_SSH_PORT: 2222
VPNS_PSK: password
VPNS_MAIN_USER: main
VPNS_MAIN_PASSWORD: password
COMMON_ADMINS_KEYS: 
  - ssh-rsa AAA... admin1@server.local
  - ssh-rsa AAA... admin2@server.local
```

Also, you can use  [ansible-vault](https://docs.ansible.com/ansible/latest/user_guide/vault.html).

## Usage

### Preparing servers
Set inventory
```commandline
export INVENTORY=default
```
To check servers availability use
To install common configuration firstboot script.
```commandline
pipenv run firstboot -l some-vpn
```
To update common configuration use common script
```commandline
pipenv run common -l some-vpn
```
To install vpn servers use vpn-servers script
```commandline
pipenv run firstboot vpn-servers -l some-vpn
```
