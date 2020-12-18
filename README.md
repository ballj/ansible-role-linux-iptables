
---
ipset_rules:
  - name: 'net_access_networks'
    description: 'access network addresses'
    type: 'hash:net'
    addresses:
      - '10.64.5.0/24'
      - '10.48.5.0/24'
      - '10.32.5.0/24'
      - '10.33.5.0/24'
      - '10.48.8.0/24'
      - '10.33.8.0/24'
  - name: 'net_ci_services'
    description: 'ci networks'
    type: 'hash:net'
    addresses:
      - '10.32.67.0/24'
  - name: 'host_ctldl_servers'
    description: 'ctldl servers'
    type: 'hash:ip'
    addresses:
      - '10.48.64.44'
  - name: 'host_dhcp_servers'
    description: 'dhcp servers'
    type: 'hash:ip'
    addresses:
      - '10.33.64.18'
      - '10.48.64.18'
      - '10.64.64.18'
      - '10.32.64.18'
iptables_rules: "\
  {{ iptables_chain_common }} + \
  {{ iptables_chain_new }}"



iptables_chain_common:
  - name: vpn
    description: 'vspc registration'
    state: 'present'
    chain: 'vpn_in'
    protocol: 'tcp'
    source_address: 1.1.1.1
    destination_address: 2.2.2.2
    destination_port: '13370'
    states: ['new','established','related']
    log: false
    action: 'accept'
iptables_chain_new:
  - name: 'vspc'
    description: 'vspc registration'
    state: 'present'
    chain: 'vpn_in'
    protocol: 'tcp'
    source_address: 172.21.0.10
    destination_address: net_ci_services
    destination_port: '13370'
    states: ['new','established','related']
    log: false
    action: 'accept'
