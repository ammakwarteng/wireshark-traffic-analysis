# Project 3: Network Traffic Analysis with Wireshark

## Overview
In this project, I captured and analyzed live network traffic on a Windows machine using Wireshark. The goal was to understand how data moves across a network, identify protocols in use, and spot security-relevant patterns in real traffic.

## Tools Used
- Wireshark (Windows)
- Host machine Wi-Fi interface

## Capture Summary
| Field | Details |
|---|---|
| Date | June 17, 2026 |
| Interface | Wi-Fi |
| Total Packets Captured | 48,026 |
| Capture File | wireshark-project3.pcap |

## Key Findings

### 1. Encrypted QUIC Traffic
The majority of traffic used the **QUIC protocol**, which is a modern encrypted transport protocol used by Google and other major services. Because it is encrypted, packet payloads showed as "Protected Payload" — this is expected and reflects good security practice by the services involved.

### 2. DNS Lookups — CDN Discovery
My machine sent DNS queries to resolve `stream-production.avcdn.net`, which is part of **Akamai's Content Delivery Network (CDN)**. This is commonly used by streaming platforms to serve media content efficiently.

**Security relevance:** DNS queries are often unencrypted, meaning an attacker on the same network could passively observe which domains a user is connecting to, even without seeing the content of those connections.

### 3. WPAD Proxy Discovery
My machine automatically sent DNS queries for `wpad.home`, which is part of the **Web Proxy Auto-Discovery (WPAD)** protocol. The server responded with "No such name," confirming no proxy is configured on the network.

**Security relevance:** WPAD can be exploited in a man-in-the-middle attack if an attacker responds to WPAD requests with a malicious proxy configuration. This is a known attack vector on corporate networks.

### 4. ICMP Destination Unreachable
Multiple **ICMP "Destination Unreachable (Port Unreachable)"** messages were observed. These occur when traffic is sent to a port that is not listening, which can indicate background application activity or misconfigured services.

## Filters Used
| Filter | Purpose |
|---|---|
| `dns` | Isolate DNS lookup traffic |
| `icmp` | View ICMP control messages |
| `quic` | View encrypted QUIC sessions |

## What I Learned
- How to capture live network traffic using Wireshark on a Windows host
- How to apply display filters to isolate specific protocol traffic
- How to read DNS query/response pairs and identify the domains being resolved
- The difference between encrypted (QUIC) and unencrypted (DNS) traffic
- Real-world security implications of WPAD and passive DNS observation

## Next Steps
- Capture HTTP traffic to observe unencrypted data in transit
- Explore the Statistics → Protocol Hierarchy view for a full breakdown
- Try capturing traffic on a VM to compare with host traffic patterns
