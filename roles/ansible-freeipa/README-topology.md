Topology modules
================

Description
-----------

These modules allow to manage the topology. That means that topology segments can be added, removed and reinitialized. Also it is possible to verify topology suffixes.


Features
--------
* Topology management


Supported FreeIPA Versions
--------------------------

FreeIPA versions 4.4.0 and up are supported by the ipatopologysegment and ipatopologysuffix modules.


Requirements
------------

**Controller**
* Ansible version: 2.8+

**Node**
* Supported FreeIPA version (see above)


Usage
=====

Example inventory file

```ini
[ipaserver]
ipaserver.test.local
```


Example playbook to add a topology segment wiht default name (cn):

```yaml
---
- name: Playbook to handle topologysegment
  hosts: ipaserver
  become: true

  tasks:
  - name: Add topology segment
    ipatopologysegment:
      password: MyPassword123
      suffix: domain
      left: ipareplica1.test.local
      right: ipareplica2.test.local
      state: present
```
The name (cn) can also be set if it should not be the default `{left}-to-{rkight}`.


Example playbook to delete a topology segment:

```yaml
---
- name: Playbook to handle topologysegment
  hosts: ipaserver
  become: true

  tasks:
  - name: Delete topology segment
    ipatopologysegment:
      password: MyPassword123
      suffix: domain
      left: ipareplica1.test.local
      right: ipareplica2.test.local
      state: absent
```
It is possible to either use the name (cn) or left and right nodes. If left and right nodes are used, then the name will be searched and used internally.


Example playbook to reinitialize a topology segment:

```yaml
---
- name: Playbook to handle topologysegment
  hosts: ipaserver
  become: true

  tasks:
  - name: Reinitialize topology segment
    ipatopologysegment:
      password: MyPassword123
      suffix: domain
      left: ipareplica1.test.local
      right: ipareplica2.test.local
      direction: left-to-right
      state: reinitialized
```
It is possible to either use the name (cn) or left and right nodes. If left and right nodes are used, then the name will be searched and used internally.


Example playbook to verify a topology suffix:

```yaml
---
- name: Playbook to handle topologysuffix
  hosts: ipaserver
  become: true

  tasks:
  - name: Verify topology suffix
    ipatopologysuffix:
      password: MyPassword123
      suffix: domain
      state: verified
```


Variables
=========

ipatopologysegment
------------------

Variable | Description | Required
-------- | ----------- | --------
`principal` | The admin principal is a string and defaults to `admin` | no
`password` | The admin password is a string and is required if there is no admin ticket available on the node | no
`suffix` | The topology suffix to be used, this can either be `domain` or `ca` | yes
`name` \| `cn` | The topology segment name (cn) is the unique identifier for a segment. | no
`left` \| `leftnode` | The left replication node string - an IPA server | no
`right` \| `rightnode` | The right replication node string - an IPA server | no
`direction` | The direction a segment will be reinitialized. It can either be `left-to-right` or `right-to-left` and only used with `state: reinitialized` | 
`state` | The state to ensure. It can be one of `present`, `absent`, `enabled`, `disabled` or `reinitialized` | yes


ipatopologysuffix
-----------------

Verify FreeIPA topology suffix

Variable | Description | Required
-------- | ----------- | --------
`principal` | The admin principal is a string and defaults to `admin` | no
`password` | The admin password is a string and is required if there is no admin ticket available on the node | no
`suffix` | The topology suffix to be used, this can either be `domain` or `ca` | yes
`state` | The state to ensure. It can only be `verified` | yes


Authors
=======

Thomas Woerner
