# pi_bond_net


This role bonds WiFi and Ethernet into single failover interface (bond0) which allows seamless transition to WiFi and back. Could be used as well to setupp fixed IP and switch over to systemd networking (as in Debian).

Role does not change password! Please keep that in mind.

Requirements
------------

Role was tested with Ansible 2.8 on Raspbian Buster. Requires `discovered_ip` variable to have IP address of Raspberry Pi that will be setup.
Best to combine it with [alxsey.ip_by_mac](https://galaxy.ansible.com/alxsey/ip_by_mac) role

Role Variables
--------------
Role supports several variables:

`discovered_ip` this variable is required. This is current IP address of Raspberry Pi to work with.

`connect_user` user that used to connect to. Raspbian is shipped with default user `pi` which is used by default.

`connect_password` password used by `connect_user`. Default is `raspberry`. Role is not changing it!

`install_ssh_key` by default role attempts to install current ssh key. Setting this variable to `false` will prohibit it.

`wifi_setup` default is tru, but setting this to false will disable setup for WiFi (useful for Raspberry Pi's that have no wifi)

`wifi_country` country code for WiFi (default `US`). Change it accordingly.

`wifi_ssid` if setting up new or changing existing SSID

`wifi_psk` again if setting up new or changing current

Dependencies
------------
None

Example Playbook
----------------

Best of all this role is used in combination with [alxsey.ip_by_mac](https://galaxy.ansible.com/alxsey/ip_by_mac) role.
In this case you can define inventory like this:
```
hosts:
  new_pi:
    ansible_host: 192.168.1.10
    main_mac:
      - 00:11:22:33:44:55
      - ff:ee:dd:cc:bb:aa
```
Playbook in this case could be:
```
- hosts: new_pi
  vars:
    default_name_servers:
      - 8.8.8.8
      - 8.8.0.0
  roles:
     - alxsey.ip_by_mac
     - alxsey.pi_bond_net
```

To use this role as standalone `discovered_ip` must be provided:
```
- hosts: another_pi
  roles:
    - role: alxsey.pi_bond_net
      vars:
        - discovered_ip: 192.168.1.2
```

## Note:
This role assumes that it will be applied to freshly installed Raspbian and to apply changes it will reboot Raspberry Pi - it also takes care of `gpu_mem` to set it to lowest possible value - 16MiB (assuming headless configuration)

License
-------

GPLv3

Author Information
------------------

Alex Ryabtsev

