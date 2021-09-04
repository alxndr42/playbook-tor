# playbook-tor

Ansible playbook for managing Tor relays on Debian-based systems.

Please see the role READMEs for requirements and variables:

* [basics](https://github.com/7adietri/ansible-basics)
* [tor](https://github.com/7adietri/ansible-tor)

The script [update-keys](update-keys) can be used to update and copy the
offline keys.

## Inventory Example

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

## Unbound

Hosts in the group `tor_unbound` are configured with a basic [Unbound][] setup.

To use it in Tor, add this to the host configuration:

    tor_nameservers: ["127.0.0.1"]

If the variable `unbound_exporter_address` is set (i.e. `localhost:9102`), a
Prometheus [unbound_exporter][] is installed.

[unbound]: https://nlnetlabs.nl/documentation/unbound/
[unbound_exporter]: https://github.com/kumina/unbound_exporter

## Prometheus

Hosts in the group `tor_exporter` are configured with a [tor_exporter][] on
`localhost:9101`.

[tor_exporter]: https://github.com/atx/prometheus-tor_exporter

## Snowflake Proxy

Hosts in the group `tor_snowflake` are configured as [Snowflake][] proxies,
using the Go binary.

[snowflake]: https://snowflake.torproject.org/

## License

GPLv3
