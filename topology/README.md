# Network topology and WAN design

The source document uses an EVE-NG lab hosted in VMware Workstation. An additional VMware NAT network (VMnet8 in the screenshots) is mapped to an EVE-NG cloud to simulate a second ISP.

## Logical traffic path

```text
 LAN clients ---------+
                      |
 DMZ clients ---------+-- Palo Alto firewall -- ethernet1/1 -- ISP 1 (simulated)
                              |
                              +------------- ethernet1/8 -- ISP 2 (VMware NAT)
```

**Diagram note:** This is a simplified logical representation of the documented interfaces, not a full cabling diagram. Source document pages 1–3, 15–21.

## Screenshot to add

- `dual-isp-topology.png` — sanitized EVE-NG topology or WAN-interface screenshot
- `vmware-nat-network.png` — VMware VMnet8 NAT setup

Remove any real credentials and sensitive management information before uploading images.

## Original multi-site EVE-NG topology captures

The two provided historical screenshots depict a broader design than the simplified dual-ISP logical drawing above.

### Screenshot 1 — Detailed topology
**Expected file:** `original-topology-detail.jpeg`

Visible elements include Sydney HQ, an Active/Passive-labelled Palo Alto pair, DMZ and Active Directory/Linux hosts, VLAN endpoints, customer-premises routing, remote VPN access, and simulated Internet links. These labels identify the intended design; the screenshot alone does not validate firewall HA or forwarding behavior.

### Screenshot 2 — Full topology overview
**Expected file:** `original-topology-overview.jpeg`

The wider topology additionally shows a London Data Center, New York Site, MPLS VPN (PE/P/PE), and customer branch devices. OSPF and RIP labels appear in separate portions of the diagram. These views are historical documentation of the lab scope rather than standalone configuration evidence.

> Image upload pending: the two original JPEG binaries need to be placed under `topology/` with the filenames above. Do not infer production deployment or verified HA results solely from the screenshots.
