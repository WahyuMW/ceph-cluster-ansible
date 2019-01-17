Ceph Deployment Using Ansible
=============================

This playbook using inventory file and roles directory with vars file in each roles. 

## Note
* This playbook will create user cephuser with no password and passwordless sudo
* Adm node will share cephuser ssh key to another node
* This playbook will deploy ceph cluster luminous release
* This playbook will isntall ceph-deploy 1.5.*
* Minimum deployment you must have 1 monitoring server, 2 OSD server, and 1 adm server

## Configuring Inventory

file : inventory

#### Node IP Addresse

```
[admin]
<admin IP address>

[manager]
192.168.220.132

[monitoring]
192.168.220.131

[osd]
192.168.220.133
192.168.220.134
192.168.220.135

[mds]

[rgw]
192.168.220.137
192.168.220.138 
```
Change IP each node with your IP node cluster. 


## Configuring Hosts Cluster

#### Configuring Hosts File

To make easy installation and easy operation in fututre use you must configure hosts file that will be use in all node. 

file : roles/node-preperation/file/hosts

```
<your ip addresse>		<Your Hosts>
192.168.220.131		monserver0
192.168.220.132		mgrserver0
192.168.220.133		osdserver0
192.168.220.134		osdserver1
192.168.220.135		osdserver2
192.168.220.137		rgw-ceph-2
192.168.220.138		rgw-ceph-1
```
Change with your ip in your cluster and each host name. This host name we will be use to in vars file. 

#### Configuring SSH Config File

You must edit this file with configuration like in host file. 
note : don't change user (We use cephuser)

file : roles/node-preperation/file/config

```
Host <your host>
        Hostname <your hostname>
        User cephuser

Host monserver0
        Hostname monserver0
        User cephuser

Host mgrserver0
        Hostname mgrserver0
        User cephuser

Host osdserver0
        Hostname osdserver0
        User cephuser

Host osdserver1
        Hostname osdserver1
        User cephuser

Host osdserver2
        Hostname osdserver2
        User cephuser
```
## Configuring Admin Varible

Change admin ceph variable

file : roles/ceph-adm/vars/main.yml

```
CEPH_NODE:
  - monserver0
  - osdserver0
  - osdserver1
  - osdserver2
  - mgrserver0
CEPH_MON:
  - monserver0
CEPH_MGR:
  - mgrserver0

CEPH_OSD_DISK:
  - osdserver0:/dev/sd{c,d,e,f}
  - osdserver1:/dev/sd{g,h,i,j,k}
  - osdserver2:/dev/sd{b,c,d}
```
Change this varible with varible you use in hosts file. 

## Running This Playbook

```
$ ansible-playbook ceph.yml -k -K
```