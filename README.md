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
user_state: present
user_aggregate:
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
system_state: present
system_hostname: switchname
system_domain_name: acme.com
system_domain_search:
  - ansible.com
  - redhat.com
system_name_servers:
  - 8.8.8.8
  - 8.8.4.4
```

Variables to test ping from the network device.

```
ping_count: 0
ping_dest: 127.0.0.1
ping_source: 127.0.0.1
ping_vrf: management
```

Variables to define logging configuration.

```
logging_aggregate:
  - dest: host
    name: 1.1.1.1
    facility: kernel
    level: critical
  - dest: host
    name: 1.1.1.2
    facility: kernel
    level: warning
logging_purge: false
logging_state: present
```

Variables to configure banner message for motd or login.

```
banner_state: present
banner: motd
banner_text: "This system management is automated by Ansible"
```

Variables to create static route.

```
static_route_state: present
static_route_purge: no
static_route_aggregate:
  - prefix: 2.2.2.2
    mask: 255.255.255.0
    next_hop: 1.1.1.1
    admin_distance: 1
```

Variables to configure L2 parameters on interface.

```
l2_interface_state: present
l2_interface_aggregate:
  - name: ethernet99/0
    mode: access
    access_vlan: 99
  - name: ethernet99/0
    mode: trunk
    native_vlan: 99
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
vlan_state: present
vlan_purge: no
vlan_aggregate:
  - name: ANSIBLEVLAN10
    state: present
    vlan_id: 10
    interfaces:
      - ethernet1/0
```

Variables to configure l3 interfaces.

```
l3_interface_state: present
l3_interface_purge: false
l3_interface_aggregate:
  - name: ethernet99/0
    ipv4: 127.0.0.1/32
```

Variables to create VRF and associate interfaces to them.

```
vrf_state: present
vrf_purge: no
vrf_aggregate:
  - name: internet
    rd: "1:204"
    interfaces:
      - ethernet1/0
```

Variables to configure low-level interface parameters.

```
interface_state: present
interface_purge: no
interface_aggregate:
  - name: ethernet99/0
    description: interface description
    enabled: yes
    mtu: 2000
    speed: 10g
```

Variables to configure link-aggregation interfaces.

```
linkagg_state: present
linkagg_purge: no
linkagg_aggregate:
  - name: group0
    members:
      - ethernet99/0
      - ethernet99/1
    mode: active
    min_links: 1
```

Variables to specify interfaces where to activate LLDP.

```
lldp_interface_state: present
lldp_interface_purge: no
lldp_interface_aggregate:
  - { name: ethernet99/0 }
```

Dependencies
------------

No dependencies from other roles.

Example Playbook
----------------

```YAML
  - name: "Baseline Configuration"
    hosts: network
    connection: local
    gather_facts: no

    tasks:
      - name: "Configure the Switch"
        include_role:
          name: victorock.network
          tasks_from: run

      - name: "Logging Configuration"
        include_role:
          name: victorock.network
          tasks_from: logging

      - name: "Banner Configuration"
        include_role:
          name: victorock.network
          tasks_from: banner

      - name: "Local Users Configuration"
        include_role:
          name: victorock.network
          tasks_from: user

      - name: "L1 interfaces"
        include_role:
          name: victorock.network
          tasks_from: interface

      - name: "L2 interfaces"
        include_role:
          name: victorock.network
          tasks_from: l2_interface

      - name: "L3 interfaces"
        include_role:
          name: victorock.network
          tasks_from: l3_interface
```

License
-------

GPLv3

Author Information
------------------

Victor da Costa, created this role to use it as part of ansible network workshops.
To have up-to-date information and materials regarding network workshops, visit
[linklight documentation](http://network-automation.github.com/linklight)
