[![Build Status](https://travis-ci.org/victorock/ansible-role-network.svg?branch=master)](https://travis-ci.org/victorock/ansible-role-network)

Role Name
=========

Network Role for net_* modules.

Requirements
------------

No specific requirements for this role.

Role Variables
--------------

For additional details regarding the variables supported by each net_* modules
consult the official [ansible documentation]( http://docs.ansible.com/ansible/latest/list_of_network_modules.html)


Variables to create users.

```
net_user:
  state: present
  aggregate:
    - name: ansible
      privilege: 15
      configured_password: ansible
      sshkey: "{{ lookup('file', '~/.ssh/id_rsa.pub') }}"
    - name: netconf
      privilege: 15
      configured_password: ansible
      sshkey: "{{ lookup('file', '~/.ssh/id_rsa.pub') }}"
```

Variables to configure system dns/hostname configuration.

```
net_system:
  state: present
  hostname: switchname
  domain_name: acme.com
  domain_search:
    - ansible.com
    - redhat.com
  name_servers:
    - 8.8.8.8
    - 8.8.4.4
```

Variables to test ping from the network device.

```
net_ping:
  count: 1
  dest: 127.0.0.1
  source: 127.0.0.1
  vrf: management
```

Variables to define logging configuration.

```
net_logging:
  aggregate:
    - dest: host
      name: 1.1.1.1
      facility: kernel
      level: critical
    - dest: host
      name: 1.1.1.2
      facility: kernel
      level: warning
  purge: false
  state: present
```

Variables to configure banner message for motd or login.

```
net_banner:
  state: present
  banner: motd
  text: "This system management is automated by Ansible"
```

Variables to create static route.

```
net_static_route:
  state: present
  purge: no
  aggregate:
    - prefix: 2.2.2.2
      mask: 255.255.255.0
      next_hop: 1.1.1.1
      admin_distance: 1
```

Variables to configure L2 parameters on interface.

```
net_l2_interface:
  state: present
  aggregate:
    - name: ethernet1/6
      mode: access
      access_vlan: 66
    - name: ethernet1/5
      mode: trunk
      native_vlan: 93
      trunk_vlans:
        - 90
        - 91
        - 92
      trunk_allowed_vlans:
        - 90
        - 91
        - 92
```

Variables to create vlan and associate interfaces to them.

```
net_vlan:
  state: present
  purge: no
  aggregate:
    - name: ANSIBLEVLAN10
      state: present
      vlan_id: 10
      interfaces:
        - ethernet1/4
    - name: ANSIBLEVLAN90
      state: present
      vlan_id: 90
    - name: ANSIBLEVLAN91
      state: present
      vlan_id: 91
    - name: ANSIBLEVLAN92
      state: present
      vlan_id: 92
    - name: ANSIBLEVLAN93
      state: present
      vlan_id: 93

```

Variables to configure l3 interfaces.

```
net_l3_interface:
  state: present
  purge: false
  aggregate:
    - name: loopback0
      ipv4: 127.0.0.1/32
```

Variables to create VRF and associate interfaces to them.

```
net_vrf:
  state: present
  purge: no
  aggregate:
    - name: control
      rd: "1:1"
      interfaces:
        - loopback0
```

Variables to configure low-level interface parameters.

```
net_interface:
  state: present
  purge: no
  aggregate:
    - name: ethernet1/3
      description: interface description
      enabled: yes
      mtu: 9000
      speed: 10g
```

Variables to configure link-aggregation interfaces.

```
net_linkagg:
  state: present
  purge: no
  aggregate:
    - name: port-channel13
      members:
        - Ethernet1/3
      mode: active
      min_links: 1
```

Variables to specify interfaces where to activate LLDP.

```
net_lldp_interface:
  state: present
  purge: no
  aggregate:
    - name: port-channel13
```

Dependencies
------------

No dependencies from other roles.

Example Playbook
----------------

```YAML
  - name: "Baseline Configuration"
    hosts: network
    connection: network_cli
    gather_facts: no
    roles:
      - role: victorock.network
        autorun: true
```

License
-------

GPLv3

Author Information
------------------

Victor da Costa, created this role to use it as part of ansible network workshops.
To have up-to-date information and materials regarding network workshops, visit
[linklight documentation](http://network-automation.github.com/linklight)
