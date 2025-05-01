# Routing cheatsheet

Created by: Tobias
Created time: May 1, 2025 5:20 PM
Last updated time: May 1, 2025 5:24 PM

# 🧰 Static and Default Route Command Summary

| Command | Description | Example |
| --- | --- | --- |
| `ip route <network> <mask> <next-hop>` | Configure a basic IPv4 static route using next-hop IP. | `ip route 192.168.2.0 255.255.255.0 10.0.0.2` |
| `ip route <network> <mask> <exit-intf>` | Configure a directly connected IPv4 static route. | `ip route 192.168.2.0 255.255.255.0 s0/0/0` |
| `ip route <network> <mask> <exit-intf> <next-hop>` | Fully specified static route for multi-access links. | `ip route 192.168.2.0 255.255.255.0 g0/0 10.0.0.2` |
| `ip route 0.0.0.0 0.0.0.0 <next-hop>` | IPv4 default route. | `ip route 0.0.0.0 0.0.0.0 10.0.0.1` |
| `ip route <host-IP> 255.255.255.255 <next-hop>` | Host route for a single IPv4 address. | `ip route 192.168.1.10 255.255.255.255 10.0.0.2` |
| `ipv6 route <prefix>/<length> <next-hop>` | Basic IPv6 static route. | `ipv6 route 2001:db8:1::/64 2001:db8:2::2` |
| `ipv6 route <prefix>/<length> <exit-intf>` | Directly connected IPv6 static route. | `ipv6 route 2001:db8:1::/64 s0/0/0` |
| `ipv6 route <prefix>/<length> <intf> <link-local>` | IPv6 static route using link-local address. | `ipv6 route 2001:db8:1::/64 s0/0/0 fe80::2` |
| `ipv6 route ::/0 <next-hop>` | IPv6 default route. | `ipv6 route ::/0 2001:db8:2::2` |
| `ping <IP>` | Tests connectivity to an IP address. | `ping 192.168.1.1` |
| `ping <IP> source <intf/IP>` | Pings using a specific source interface. | `ping 192.168.1.1 source g0/0` |
| `traceroute <IP>` | Shows the path a packet takes. | `traceroute 192.168.1.1` |
| `show ip route` | Displays the IPv4 routing table. | — |
| `show ipv6 route` | Displays the IPv6 routing table. | — |
| `show run | ip route` | Filters config to show only static IPv4 routes. | — |
| `show ip interface brief` | Quick interface and IP overview. | — |
| `show cdp neighbors` | View directly connected Cisco devices. | — |