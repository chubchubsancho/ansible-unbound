# ansible-unbound

[![Build Status](https://travis-ci.com/chubchubsancho/ansible-unbound.svg?branch=master)](https://travis-ci.com/chubchubsancho/ansible-unbound)
[![License](https://img.shields.io/badge/license-MIT-blue.svg?logo=github&style=flat)](https://raw.githubusercontent.com/chubchubsancho/ansible-unbound/master/LICENSE)

An ansible role to install and configure unbound

## Role configuration

* `unbound_root_hints_url` (default: "https://www.internic.net/domain/named.cache") - define
  `root-hints` url
* `unbound_root_hints_dir` (default: "/etc/unbound/root.hints") - define
  `root-hints` install directory
* `unbound_conf_dir` (debian default: "/etc/unbound/unbound.conf.d") - define
  `unbound` custom config directory

## Define unbound global configuration

Add `unbound` variable for global config thes vars can be omited

* `unbound_log_dir` (default: "/var/log/unbound") - define `unbound` log directory
* `unbound_verbosity` (default: "1") - define `unbound` verbosity
* `unbound_log_time_ascii` (default: "yes") - define if log time must be ascii
* `unbound_log_queries` (default: "yes") - define if we must log queries
* `unbound_log_relpies` (default: "yes") - define if we must log replies
* `unbound_log_tag_queryreply` (default: "yes") - define if we must tag logs queries/replies
* `unbound_local_actions`: (default: "yes") - define if we must log local actions
* `unbound_servfail`: (default: "yes") - define if we must log SERVFAIL
* `unbound_interface`: (default: "127.0.0.1") - define `unbound` listen interface
* `unbound_port`: (default: "5053") - define `unbound` listen port
* `unbound_do_tcp`: (default: "yes") - define if `unbound` will do tcp
* `unbound_do_udp`: (default: "yes") - define if `unbound` will do udp
* `unbound_do_ip4`: (default: "yes") - define if `unbound` will do ipv4
* `unbound_do_ip6`: (default: "yes") - define if `unbound` will do ipv6
* `unbound_harden_dnssec_stripped`
* `unbound_prefetch`: (default: "yes") - define if we must prefetch cached records
* `unbound_private_addresses`:
  (default: "[192.168.0.0/16,169.254.0.0/16,172.16.0.0/12,10.0.0.0/8,"fd00::/8","fe80::/10"]") -
  define unbound private address

## Define unbound local-data configuration

* name: domaine name
* mode: mode for this local zone
* access_control: define who can query this zone
  * allow
  * deny
* local-data: add records for this zone
  * name: record name
  * type: record type
  * ttl: record ttl
  * ptr: does we need to create ptr record

## Examples

### vars file

```yaml
---
unbound_domain:
  - name: foo.bar
    mode: static
    access_control:
      - allow:
          - 192.168.0.0/24
          - 10.0.0.0/8
      - deny:
          - 192.168.1.0/24
    local_data:
      - name: first
        type: A
        ttl: 30
        ip: 192.168.0.1
        ptr: true
      - name: second
        type: A
        ttl: 30
        ip: 192.168.0.2
  - name: bar.foo
    mode: static
    access_control:
      - allow:
          - 192.168.0.0/24
      - deny:
          - 192.168.1.0/24
          - 10.0.0.0/8
    local_data:
      - name: first
        type: A
        ttl: 30
        ip: 192.168.0.3
      - name: second
        type: A
        ttl: 30
        ip: 192.168.0.4
        ptr: true
```
