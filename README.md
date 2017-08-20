playbook-tor
============

Ansible playbook for managing Tor relays on Debian-based systems.

Please see the role READMEs for requirements and variables:

* [basics](https://github.com/7adietri/ansible-basics)
* [tor](https://github.com/7adietri/ansible-tor)

The script [update-keys](update-keys) can be used to update and
copy the offline keys.

Inventory Example
-----------------

A minimal set of variables for a middle relay:

    basics_autoupdate_origins:
      - o=TorProject,n=${distro_codename}
    tor_instances_middle:
      - name: relay
        nickname: MyCoolRelay
        contact_info: "Foo <foo@example.com>"
        or_ports:
          - 443
          - "[abcd::1:2:3:4]:443"

License
-------

GPLv3
