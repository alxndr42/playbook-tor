# playbook-tor

Ansible playbook for managing Tor relays on Debian-based systems.

The script [update-keys](update-keys) can be used to update and copy the
offline keys.

Please see the individual roles for more information and defaults.

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

## Exit Notice

Hosts in the group `tor_relays` that have `tor_instances_exit` are configured
with nginx and a `tor-exit-notice` site for all exit `outbound_addresses`,
unless `nginx` and `tor_exit_notice` are set to `false`.

Copyright for the [sample exit notice][] is held by The Tor Project, Inc.

[sample exit notice]: roles/tor-exit-notice/files/tor-exit-notice/index.html

## Unbound

Hosts in the group `tor_relays` that have `tor_instances_exit` are configured
with [Unbound][], unless `unbound` is set to `false`.

To use it in Tor, add this to the host configuration:

```yaml
tor_nameservers: ["127.0.0.1"]
```

If `unbound_exporter_address` is defined (i.e. `address:port`), a Prometheus
[unbound_exporter][] is installed.

[unbound]: https://unbound.docs.nlnetlabs.nl/
[unbound_exporter]: https://github.com/letsencrypt/unbound_exporter

## Prometheus Tor Exporter

Hosts in the group `tor_relays` are configured with nginx and [tor_exporter][],
unless `nginx` and `tor_exporter` are set to `false`.

The variables `tor_exporter_bridge`, `tor_exporter_middle` and
`tor_exporter_exit` should contain the configuration for exporter instances:

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
