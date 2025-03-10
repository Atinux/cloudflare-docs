---
_build:
  publishResources: false
  render: never
  list: never
---

Use this selector to apply policies to DNS queries that arrived to your Gateway Resolver IP address aligned with a registered DNS location. For most Gateway customers, this is an IPv4 anycast address and policies created using this IPv4 address will apply to all DNS locations. However, each DNS location has a dedicated IPv6 address and some Gateway customers have been supplied with a dedicated IPv4 address — these both can be used to apply policies to specific registered DNS locations.

| UI name         | API example                               | Evaluation phase      |
| --------------- | ----------------------------------------- | --------------------- |
| DNS Resolver IP | `any(dns.resolved_ip[*] == 198.51.100.0)` | Before DNS resolution |
