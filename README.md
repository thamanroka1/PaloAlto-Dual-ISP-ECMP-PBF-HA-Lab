# Palo Alto Networks | Dual-ISP Routing, ECMP, PBF & HA Design

**Enterprise network-security lab · PAN-OS · EVE-NG · VMware Workstation**

A hands-on firewall project exploring resilient internet connectivity, session-aware traffic steering, outbound NAT, static-route monitoring, and Palo Alto Networks high-availability architecture.

> **Evidence boundary:** Dual-ISP, NAT, ECMP, route monitoring, and PBF are documented with configuration screenshots and selected connectivity checks. HA is discussed as a design/reference topic; measured HA failover is **not** claimed.

## At a glance

| Area | What was explored | Evidence |
| --- | --- | --- |
| Dual WAN | Two simulated upstream ISP connections and source NAT | Screenshots in `screenshots/` |
| Static routing | Default routes with preferred metrics | Routing table examples |
| ECMP | Equal-cost eligible paths and distribution algorithms | Configuration and ping/traffic-log examples |
| Failover | Static-route path monitoring and backup path | Monitor configuration and CLI check |
| PBF | Source-based egress selection for LAN/DMZ traffic | Policy examples and hit/traffic logs |
| HA | Active/passive vs. active/active, HA1/HA2 and monitoring | Reference/design material only |

## Original EVE-NG topology screenshots

The two original screenshots supplied for this project show a **larger multi-site security lab** that includes the Sydney HQ, two Palo Alto firewalls labelled HA-Active and HA-Passive, a DMZ, VLAN-based LAN segments, customer branches, a remote VPN user, a London data center, a New York site, and an MPLS VPN area. These images provide historical topology context; they are not proof that every displayed security, routing or HA feature passed verification.

| Screenshot | View | Description |
| --- | --- | --- |
| `topology/original-topology-detail.jpeg` | Detail | Sydney HQ, HA-labelled firewall pair, DMZ, VLANs, routing segments and remote access connectivity |
| `topology/original-topology-overview.jpeg` | Overview | Wider EVE-NG view of Sydney HQ, London Data Center, New York Site and MPLS VPN |

The images are available in the original project capture. **Their binary image files must be uploaded separately to the paths above before image previews will render.** The original dual-ISP study remains documented independently below.

See [topology notes](topology/README.md).

## Lab environment

- **Firewall:** Palo Alto Networks PAN-OS (exact version not recorded)
- **Emulation:** EVE-NG
- **Virtual networking:** VMware Workstation; a VMware NAT network provides the second simulated ISP
- **Test segments:** LAN and DMZ clients
- **Routing:** Static default routes, ECMP, PBF and path monitoring

### Logical topology

```text
 LAN clients ----+
                 +---- Palo Alto firewall ---- ethernet1/1 ---- ISP 1 (simulated)
 DMZ clients ----+             |
                               +---- ethernet1/8 ---- ISP 2 (VMware NAT)
```

This is a **logical** diagram, not a verified physical cabling plan. See [topology notes](topology/README.md).

## How traffic flows

1. A LAN or DMZ client sends traffic to its gateway.
2. The firewall evaluates applicable security and forwarding policies.
3. PBF, when matched and applicable, can steer traffic to a specified egress/next hop; otherwise routing-table decisions apply. ECMP can distribute eligible sessions over equal-cost paths.
4. Source NAT is applied as configured for the selected outbound path.
5. The firewall forwards traffic and tracks the session; traffic logs and policy counters help verify behavior.

**Important:** ECMP distributes eligible sessions; it does not necessarily split each packet equally across ISPs. PBF is distinct from ECMP. Security-policy authorization is not the same as user authentication.

## Implementation walkthrough

### 1 — Dual ISP and outbound NAT
Configure two WAN-facing interfaces, assign the correct zones, and create source NAT rules for outbound traffic via each simulated ISP. Confirm sessions and translations in traffic logs.

### 2 — Static routes and ECMP
Add `0.0.0.0/0` routes toward both upstream gateways. A lower route metric makes a path preferred; equal-cost eligible routes can participate in ECMP when enabled. The source material discusses IP Modulo, IP Hash, Balanced Round Robin and Weighted Round Robin.

### 3 — Primary/backup path monitoring
Configure path monitoring to detect upstream reachability failure beyond the local interface, withdraw the affected static route when its monitor fails, and use an eligible backup route. Recovery depends on monitoring and hold-time configuration.

```text
show routing path-monitor
```

### 4 — Policy-Based Forwarding
Create policies matching traffic sources such as LAN and DMZ, set intended egress interface and next-hop, and test policy matches, connectivity, and monitor/backup behavior. PBF can override normal destination-based routing for matching sessions, subject to configuration.

### 5 — High Availability architecture (design study)
Review active/passive and active/active designs, HA1 control and HA2 data links, heartbeats, state/config synchronization, and link/path monitoring. **No measured HA failover times or full end-to-end HA tests are provided.**

## Verification and evidence

| Check | What is documented | Interpretation |
| --- | --- | --- |
| Default routes | Routing-table screenshots | Shows route configuration/state at capture time |
| Connectivity | Client pings to `8.8.8.8` and `4.2.2.2` | Demonstrates reachability at capture time |
| ECMP | Configuration and traffic logs | Illustrates session/path selection |
| Route monitoring | Path-monitor configuration and status | Illustrates monitored failover design |
| PBF | Policies, counters, traffic logs | Shows traffic-steering checks |
| HA failover | No measured results | **Not verified** |

See the [verification guide](verification/README.md) and [screenshots directory](screenshots/). Screenshot filenames preserve source-page numbers; some may contain handwritten annotations from the original documentation. Images should be reviewed and sanitized before being reused outside this portfolio.

## Authentication and security scope

This project focuses on **network traffic policy and forwarding**, not an application login system. The repository does **not** demonstrate a user sign-in workflow, credential store, API-token issuance, or token rotation. Firewall administration and API authentication are not documented here, so their configuration must not be inferred.

**Publication hygiene:** Never commit real passwords, API keys, private certificates/keys, management secrets, unredacted sensitive addressing, or proprietary PAN-OS software images. Review screenshots and config exports before publication; rotate any credential accidentally exposed.

## Repository map

```text
.
├── README.md                  # Project overview and technical narrative
├── topology/README.md         # Logical topology and WAN design
├── verification/README.md     # Evidence mapping and limitations
└── screenshots/               # Page-numbered source-document images
```

## Known limitations / next improvements

- [ ] Record exact PAN-OS, EVE-NG and VMware versions
- [ ] Replace page-numbered image names with descriptive filenames and captions
- [ ] Sanitize and peer-review all screenshots before broader distribution
- [ ] Add redacted interface, NAT, routing and PBF configuration exports
- [ ] Record repeatable test cases with dates, expected results and observed outputs
- [ ] Validate HA with device-state logs and measured failover/recovery times
- [ ] Add failure-mode testing for ISP outage, monitor failure and path recovery

## Skills demonstrated

**Palo Alto Networks · PAN-OS · Network Security · NAT · Dual WAN · Static Routing · ECMP · Policy-Based Forwarding · Path Monitoring · Troubleshooting · EVE-NG · VMware Workstation**

---

*Portfolio documentation of a simulated lab. This repository is not a production deployment guide, and all results are limited to the evidence captured in the source material.*
