VPN IPSec
=========

Ansible role to manage VPN IPsec Config with Strongswan

Requirements 
------------

Ansible 2.3 or  higher


Ansible role to manage IPSec config and it's scripts
to test VPN endpoints.

Example of Var File
------------

```
---
#if you use prometheus you can set this variable
vpn_ipsec_pushgateway_url: []

ipv4_forward: "yes"

vpn_ipsec_global_config: |
    uniqueids=no
    strictcrlpolicy=yes
    charondebug="cfg 4, ike 4, esp 4, chd 4, mgr 4, net 4"

# newtwork config for instance
vpn_subnet_staging: "192.168.0.0/24"
vpn_ipsec_public_ip: 8.8.8.8
vpn_ipsec_private_ip: 192.168.0.1

# vpn client configuration
vpn_client_connections:
  - name: client-vpn
    state: present
    test_host: 192.168.1.1
    test_port: 1433
    subnet: 192.168.0.0/24
    public_ip: 8.8.8.8
    secret: "{{ vault_psk_client.client_vpn }}"
    config: |
        #phase 1
        ike=aes128-sha1-modp1024
        keyexchange=ikev1
        ikelifetime=1h
        #phase 2
        esp=aes128-sha1-modp1024
        lifetime=1h
        left=
        leftid=
        leftsubnet=
        right=
        rightsubnet=
        #general options
        dpddelay=30
        dpdtimeout=5
        dpdaction=restart
        authby=secret
        auto=start
        type=tunnel
        leftfirewall=yes
        mobike=no
```
