sbog/openvpn
============

Role to install and configure OpenVPN server

#### Requirements

Ansible 2.4

#### Role Variables

```yaml
# Filename for target server keypair file. It can be useful in case
# you already had some OpenVPN PKI and now just want to migrate to the new
# version, so if you had OpenVPN server keys named after 'some.name.[key,crt]'
# and will set server_name var to 'some.name', your keys won't be regenerated
# and you will continue to use old ones.
server_name: localhost

# Hostname which clients should connect to. In case you will
# use it, exactly that name will be shown in client ovpn file as server to
# connect to, otherwise 'server_name' var will be used. Optional.
#real_server_name: 100.100.100.100

# List of additional pushes which will be advertised for
# clients.
additional_pushes:
 - "redirect-gateway def1"
# Server address and type
server_port: 1494
protocol: udp
# Valid values: tun, tap
dev_type: tun
# If False, ta.key won't be included
use_tls_auth: True
# Public address to bind. Can be specified if needed to bind to exact intervace
# address_to_bind: 127.0.0.1
vpn_network: 10.10.0.0
vpn_netmask: 255.255.255.0
vpn_cidr: 24
dns_pushes:
  - 8.8.8.8
  - 8.8.4.4
ping: 5
ping_restart: 30
syslog_name: openvpn
admin_email: admin@localhost
# What to do with given client name. Valid values: issue, revoke
keys_action: issue
iptables_configure: True
# List of clients to issue certificate for. In case of first-time
# installation it will be ignored due to way revokation list created. Next
# times it will be used as a name of client to create certificate for.
# Default clients list is empty
clients: []
```

#### Dependencies

None

#### Example Playbook

```yaml
- name: Install VPN server and create some client certs for it
  hosts: vpn-server
  remote_user: root
  sudo: yes
  vars_prompt:
    - name: "clients"
      prompt: "Please enter login name"
      private: no
  roles:
    - vpn
```

#### License

Apache 2.0

#### Author Information

Stanislaw Bogatkin (https://sbog.ru)
