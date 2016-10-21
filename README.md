## Deploy docker-registry with Ansible

### Clone github repo
```git clone https://github.com/oisis/ansible-docker-registry```

Enter into ansible directory:

```cd ./ansible-docker-registry```

### Vagrant

#### Check available hosts on vagrant:
```vagrant status```

#### Run on Vagrnat:
```vagrant up docker-registry```

#### Provision machine after changes
```vagrant provision docker-registry```

### Bare metal/virtual host

#### Setup remote host ssh credential settings
First you need to edit file [hosts](https://github.com/oisis/ansible-docker-registry) and change IP for prod-docker-registry.
Change 192.168.0.254 to your server real IP address

#### Run ansible connection test:
```ansible -i ./hosts all --limit prod-docker-registry -m ping```

#### Check what Ansible is going to do:
```ansible-playbook -i ./hosts --limit docker-registry playbook.yaml --check```

#### Run Ansible on remote host prod-docker-registry:
```ansible-playbook -i ./hosts -u username -k --limit prod-docker-registry playbook.yaml```

- -u - user name for ssh connection
- -k - ask password for ssh username
- -i - file with hosts list
- --limit - run only on host from group
- playbook.yaml - playbook name to run
