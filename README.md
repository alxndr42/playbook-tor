# playbook-tor

Ansible playbook for managing Tor relays on Debian-based systems.

The script [update-keys](update-keys) can be used to update and copy the
offline keys.

Please see the individual roles for more information and default values.

External collections/roles:

- [commons](https://github.com/alxndr42/ansible-commons)
- [tor](https://github.com/alxndr42/ansible-tor)
- [unbound](https://github.com/alxndr42/ansible-unbound)

## Requirements

- Debian-based system with systemd
- Offline keys as described in the `tor` role
- Dependencies installed

```bash
# Install dependencies via GitHub
ansible-galaxy install -r requirements-github.yml
```

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

## Exit Notice

Hosts in the group `tor_relays` that have `tor_instances_exit` are configured
with nginx and a `tor-exit-notice` site for all exit `outbound_addresses`,
unless `tor_exit_notice` is set to `false`.

Copyright for the [sample exit notice][] is held by The Tor Project, Inc.

[sample exit notice]: roles/tor-exit-notice/files/tor-exit-notice/index.html

## Tor Exporter (Deprecated)

Hosts in the group `tor_relays` are configured with a Prometheus
[tor_exporter][], if `tor_exporter` is set to `true`.

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

GNU General Public License v3 or later (GPLv3+)
