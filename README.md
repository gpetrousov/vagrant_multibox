# Vagrant Multibox

This is a multibox vagrant template which you can use to spawn multiple VMs.

## Connectivity

You can connect from your host to any guest using `vagrant ssh`. But, you can also connect
from the "master" VM to any "node" VM using SSH key-pairs.

## SSH key-pairs

The SSH keys are generated and added automatically to the VMs each time you delete them from
the files directory.

## How to Use

1. Create the environment
```
$ vagrant up
Key-pair not found
Bringing machine 'master' up with 'virtualbox' provider...
Bringing machine 'node0' up with 'virtualbox' provider...
Bringing machine 'node1' up with 'virtualbox' provider...
...
```

2. Check if your environment is created
```
$ vagrant status
Current machine states:

master                    running (virtualbox)
node0                     running (virtualbox)
node1                     running (virtualbox)
```

3. Connect to master VM

```
$ vagrant ssh master
```

4. SSH into any node using the IPs from Vagrantfile

```
$ vagrant ssh master
[vagrant@master ~]$ ssh 192.168.33.12
The authenticity of host '192.168.33.12 (192.168.33.12)' can't be established.
ECDSA key fingerprint is SHA256:Di6tNR3KiABiJCcNtfeYGsy6Pgd6ByNMKxP9PYzSwig.
ECDSA key fingerprint is MD5:16:2c:44:9e:00:be:28:a2:3c:61:69:ca:8b:f0:0b:12.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '192.168.33.12' (ECDSA) to the list of known hosts.
[vagrant@node0 ~]$
```

5. Profit
```
[vagrant@master ~]$ ssh 192.168.33.12
[vagrant@node0 ~]$
```



## Use cases
- DCOS playground: bootstrap, master, agents configuration
- Kubernetes playground: master, agents configuration
- Docker playground: multihost configuration
- Ansible playground: master, agents configuration
- Jenkins playground: master, agents configuration
- Any system which requires master-slave communication based on SSH

## Contributing
Contributions are open through a PR.
