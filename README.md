# playbook-tor

Ansible playbook for managing Tor relays on Debian-based systems.

Please see the role READMEs for requirements and variables:

- [basics](https://github.com/alxndr42/ansible-basics)
- [tor](https://github.com/alxndr42/ansible-tor)

The script [update-keys](update-keys) can be used to update and copy the
offline keys.

## Inventory Example

A minimal set of variables for a middle relay:

```yaml
basics_autoupdate_origins:
  - o=TorProject,n=${distro_codename}

tor_contact_info: "Foo <foo@example.com>"
tor_instances_middle:
  - name: middle01
    nickname: MiddleRelay01
    or_ports:
      - 443
      - "[abcd::1:2:3:4]:443"
```

## Unbound

Hosts in the group `unbound` are configured with a basic [Unbound][] setup.

To use it in Tor, add this to the host configuration:

```yaml
tor_nameservers: ["127.0.0.1"]
```

If the variable `unbound_exporter_address` is set to `address:port`, a
Prometheus [unbound_exporter][] is installed.

[unbound]: https://unbound.docs.nlnetlabs.nl/
[unbound_exporter]: https://github.com/letsencrypt/unbound_exporter

## Prometheus

Hosts in the groups `tor_exporter_bridge`, `tor_exporter_middle` and
`tor_exporter_exit` are configured with a [tor_exporter][]:

```yaml
tor_exporter_middle:
  - name: middle01
    control_socket: /var/run/tor-instances/middle01/control
    user: _tor-middle01
    bind_port: 9101
```

[tor_exporter]: https://github.com/atx/prometheus-tor_exporter

## Snowflake Proxy

Hosts in the group `tor_snowflake` are configured as [Snowflake][] proxies,
using the Go binary.

[snowflake]: https://snowflake.torproject.org/

## License

GPLv3
