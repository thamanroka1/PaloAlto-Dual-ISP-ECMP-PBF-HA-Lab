# Verification evidence

The source document includes screenshots of these lab checks:

| Test | Evidence in source document | Suggested image filename |
|---|---|---|
| Two ISP default routes | Page 4, routing-table screenshots | `static-routes.png` |
| ECMP configuration | Page 7, ECMP configuration and equal-cost routes | `ecmp-config.png` |
| Client connectivity | Page 8, successful pings to 8.8.8.8 and 4.2.2.2 | `client-ping-tests.png` |
| Firewall traffic monitoring | Page 8, traffic logs | `traffic-logs.png` |
| Static-route path monitoring | Pages 12–14, monitor configuration and forwarding table | `route-path-monitor.png` |
| PBF forwarding and hit counts | Pages 15–18, PBF rules, client ping, traffic logs | `pbf-verification.png` |
| PBF monitor profiles | Pages 19–20 | `pbf-monitoring.png` |

## Verification caveats

- The document shows configuration screens and selected connectivity results, not a reproducible timestamped test suite.
- Screenshots of HA concepts do not by themselves establish measured HA failover.
- Some explanatory notes use broad language about ECMP/PBF; behavior depends on PAN-OS configuration and version.
- Keep lab screenshots sanitized before public upload.

## Useful PAN-OS command

```text
show routing path-monitor
```

Use this to inspect path-monitoring state on supported PAN-OS configurations.
