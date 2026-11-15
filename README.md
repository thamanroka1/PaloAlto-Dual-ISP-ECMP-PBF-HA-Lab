# Palo Alto Networks — Dual ISP, ECMP, PBF & HA Lab

Hands-on Palo Alto Networks firewall lab documentation covering dual-ISP routing, NAT, Equal-Cost Multi-Path (ECMP), policy-based forwarding (PBF), path monitoring, and high-availability concepts.

## Lab environment

- Palo Alto Networks firewall (PAN-OS)
- EVE-NG virtual network lab
- VMware Workstation virtual networks to simulate separate ISP connections
- LAN and DMZ test clients

> **Scope:** The dual-ISP, ECMP, static-route monitoring, and PBF sections include configuration and test screenshots. The later high-availability material includes configuration concepts and reference diagrams; it should not be interpreted as proof that every HA scenario was fully tested.

## 1. Dual ISP connectivity and NAT

The lab uses separate VMware virtual networks as simulated upstream ISP connections. An additional EVE-NG cloud interface connects the firewall to the second simulated ISP.

- Configure the second WAN interface and IP addressing.
- Assign a security zone appropriate to the design.
- Configure source NAT for outbound traffic on each ISP.
- Verify traffic using the firewall's session/traffic logs.

## 2. Static default routes and ECMP

Two default routes (`0.0.0.0/0`) point to separate ISP gateways.

- With different metrics, the lower-metric route is preferred.
- With ECMP enabled and equal-cost eligible routes, the firewall can distribute sessions across both WAN paths.
- The documentation compares IP Modulo, IP Hash, Balanced Round Robin, and Weighted Round Robin methods.

**Verification:** The lab records successful client ping tests and firewall traffic logs while examining path selection.

## 3. Primary/backup ISP failover

The lab demonstrates primary and secondary default routes with different metrics and route path monitoring.

- Monitor reachability beyond the local firewall interface.
- Withdraw an unusable primary route when its monitored destination becomes unreachable.
- Use the secondary route while the primary path is unavailable.
- Check routing and monitoring state with:

```text
show routing path-monitor
```

Recovery behavior depends on monitoring and preemptive hold-time settings.

## 4. Policy-Based Forwarding (PBF)

The lab explores steering different traffic sources over selected ISP paths.

- Define PBF rules for LAN and DMZ traffic.
- Specify the egress interface and next-hop gateway.
- Add PBF monitoring and backup behavior.
- Verify client connectivity, policy hit counts, and firewall traffic logs.

PBF is a policy-based traffic-steering mechanism; it is not the same as ECMP load balancing.

## 5. Palo Alto High Availability (HA)

The documentation also discusses firewall HA design:

- Active/passive and active/active modes
- HA1 control and HA2 data links
- Heartbeats, session/configuration synchronization
- Link and path monitoring
- Failover design considerations

**Note:** HA design/reference material is included separately from the dual-ISP test evidence. Validate HA functionality with device state and failover logs before claiming end-to-end HA testing.

## Verification checklist

- [x] Document dual-ISP interface and NAT setup
- [x] Document equal-cost routing / ECMP configuration
- [x] Capture client connectivity tests and firewall traffic logs
- [x] Document primary/backup path-monitoring setup
- [x] Document PBF policy and monitoring examples
- [ ] Add sanitized, independently labeled screenshots to this repository
- [ ] Add measured HA failover results and logs
- [ ] Add exact PAN-OS version and sanitized device configuration exports

## Repository safety

Do not commit real passwords, API keys, management credentials, unredacted public IP information, or licensed PAN-OS images. All lab addresses and examples should be reviewed before reuse in production.

## Project purpose

Demonstrate practical enterprise network-security skills in Palo Alto firewall routing, dual-WAN traffic management, monitoring, redundancy, and troubleshooting.
