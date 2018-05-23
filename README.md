# DHCP role for ansible.

This is a Ansible role for configuring  DHCP `dhcpd.conf`


## Variables

Here is an example variables file

```
---
dhcpd:
  globals:
    options:
      domainname: "dhcp.default"
  subnets:
    - ip: 10.0.1.0
      netmask: 255.255.255.0

    - ip: 10.0.2.0
      netmask: 255.255.255.0
      options:
        routers: 10.0.2.1
        subnet-mask: 255.255.255.0
        domain-name-servers: 8.8.8.8
      ranges:
        - 10.0.2.2 10.0.2.100
```

## Handlers

This role provides two handlers

* `start-dhcpd`
* `reload-dhcpd`

## Example Task with role

```
---
  - name: DHCP | Install DHCP Server
    yum:
      name: dhcp
      state: present

  - name: DHCP | Write configuration file
    template:
      src: dhcpd.conf.j2
      dest: /etc/dhcp/dhcpd.conf
      owner: root
      group: root
      mode: 0644
    notify: reload-dhcpd
    with_dict: "{{ dhcpd }}"
```
