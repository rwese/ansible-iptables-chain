# rwese/ansible-iptables-chain

Create and maintain an iptables chain within an existing rules environment.

This can work together with docker and kubernetes as it preservse existing
rules and keep existing connections.

It will create an systemd service which runs on start and create the chain at
the first position of INPUT `iptables_chain__interface`.

## Requirements

- iptables
- systemd

## Role Variables

| Variable Name                 | Type   | Purpose                                                | Default                                             | Required |
| ----------------------------- | ------ | ------------------------------------------------------ | --------------------------------------------------- | -------- |
| `whitelisted_cidrs`           | Array  | Whitelisted cidrs                                      | RFC1918                                             | Yes      |
| `iptables_chain__state`       | String | If not present chain will be removed                   | `present`                                           | Yes      |
| `iptables_chain__interface`   | String | Only follow the chain when for this incoming interface | `eth0`                                              | Yes      |
| `iptables_chain__log_dropped` | String | log dropped packets, recommended for initial setup     | `true`                                              | Yes      |
| `iptables_chain__dry_run`     | String | will only log                                          | `true`                                              | No       |
| `iptables_chain__name`        | String | if you run multiple chains they must be unique         | `FILTER-INPUT-SECURE`, must not contain whitespaces | Yes      |
| `rules`                       | String |                                                        | `[]`                                                | No       |

## Dependencies

none

## Example Playbook

    - hosts: servers
      become: true
      gather_facts: true
      roles:
        - role: ansible-iptables-chain
          vars:
            iptables_chain__dry_run: true
            iptables_chain__log_dropped: true
            whitelisted_cidrs:
            # your public ip
              - 80.1.2.3
              - 10.0.0.0/24
            rules:
              - protocol: tcp
                port: 22
              - protocol: tcp
                ip_version: 4
                port: 20
              - protocol: tcp
                ip_version: 6
                port: 21
              - protocol: tcp
                port: 80
              - protocol: tcp
                port: 443

## License

MIT

## Author Information

This role is primary for personal use and no support is promised
