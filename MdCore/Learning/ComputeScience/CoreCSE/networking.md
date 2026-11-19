
# SECTION 12 · NETWORKING

## 12.1 Models & Layers

### OSI Model

**7 layers:**
| Layer | Name | Function | Examples |
|-------|------|----------|----------|
| 7 | Application | User services | HTTP, FTP, SMTP |
| 6 | Presentation | Data format | TLS, JPEG |
| 5 | Session | Dialog control | RPC, NetBIOS |
| 4 | Transport | End-to-end | TCP, UDP |
| 3 | Network | Routing | IP, ICMP |
| 2 | Data Link | Framing | Ethernet, WiFi |
| 1 | Physical | Bits | Cables, radio |

**Mnemonic:** *Please Do Not Throw Sausage Pizza Away.*

### TCP/IP Model

**4 layers:**
| Layer | Equivalent OSI |
|-------|----------------|
| Application | 5–7 |
| Transport | 4 |
| Internet | 3 |
| Link | 1–2 |

**Practical model:** Internet uses TCP/IP.

**Recall:** *Practical internet model.*

### Physical Layer

**Media:** Copper (twisted pair, coax), fiber (single/multi-mode), wireless (radio, microwave, infrared).

**Encoding:** NRZ, Manchester, 4B/5B.

**Bandwidth vs latency:** Bandwidth = bits/sec; latency = delay.

**Recall:** *Bits on wire.*

### Data Link Layer

**Framing:** Delimit frames; detect errors.

**MAC:** Media Access Control; 48-bit address.

**Error detection:** Parity, checksum, CRC.

**Protocols:** Ethernet, WiFi (802.11), PPP.

**Recall:** *Node-to-node.*

### Ethernet

**Standard:** IEEE 802.3.

**Frame:** Preamble, dest MAC, src MAC, type, payload, FCS.

**Speeds:** 10 Mbps → 100 Mbps → 1 Gbps → 10 Gbps → 100 Gbps.

**Switches:** Forward frames based on MAC table.

**Recall:** *Wired LAN.*

---

## 12.2 Network Layer

### IP Addressing

**IPv4:** 32-bit; dotted decimal (192.168.1.1).

**IPv6:** 128-bit; hex colon-separated.

**Subnetting:** Divide network into subnets.

**CIDR:** Classless Inter-Domain Routing; /24 = 256 addresses.

**Public vs private:** Private ranges (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16).

**Recall:** *Address of host.*

### Routing

**Static:** Manual routes.

**Dynamic:** Protocols exchange routes.
- **RIP:** Distance vector; hop count
- **OSPF:** Link state; Dijkstra
- **BGP:** Path vector; inter-AS

**Routing table:** Dest, next hop, interface, metric.

**Recall:** *Path selection.*

### BGP Details

**Autonomous System (AS):** Network under single admin.

**BGP:** Exchanges reachability between ASes.

**Path selection:** Local pref, AS path length, MED, etc.

**Policy:** Routing based on business relationships (peering, transit).

**Recall:** *Inter-AS routing.*

### MPLS

**Definition:** Multi-Protocol Label Switching; labels instead of IP lookup.

**Benefits:** Fast forwarding; traffic engineering; VPNs.

**Recall:** *Label switching.*

### SDN (Software-Defined Networking)

**Definition:** Separate control plane from data plane.

**Controller:** Centralized; programs forwarding.

**OpenFlow:** Protocol between controller and switches.

**Recall:** *Programmable networks.*

### NFV (Network Function Virtualization)

**Definition:** Run network functions (firewall, NAT) as software.

**VNF:** Virtual Network Function.

**Benefits:** Flexibility, cost.

**Recall:** *Virtual network functions.*

### NAT

**Definition:** Network Address Translation; map private to public IP.

**Types:**
- **Static:** 1:1 mapping
- **Dynamic:** Pool of public IPs
- **PAT (Port Address Translation):** Many private → one public via ports

**Recall:** *Share one public IP.*

### Firewalls

**Packet filter:** Inspect headers; allow/deny.

**Stateful:** Track connections.

**Application:** Deep packet inspection.

**DMZ:** Demilitarized zone; public-facing services.

**Recall:** *Traffic filter.*

---

## 12.3 Transport Layer

### TCP

**Properties:**
- Connection-oriented
- Reliable
- Ordered
- Flow control
- Congestion control

**Handshake (3-way):**
```
Client → SYN → Server
Client ← SYN-ACK ← Server
Client → ACK → Server
```

**Flow control:** Sliding window; receiver advertises window.

**Congestion control:**
- **Slow start:** Exponential growth
- **Congestion avoidance:** Linear growth
- **Fast retransmit:** 3 duplicate ACKs
- **Fast recovery:** Halve cwnd

**Algorithms:** Tahoe, Reno, CUBIC, BBR.

**Recall:** *Reliable stream.*

### UDP

**Properties:**
- Connectionless
- Unreliable
- Unordered
- No flow/congestion control

**Use cases:** DNS, DHCP, streaming, gaming, VoIP.

**Recall:** *Fire and forget.*

### Sockets

**Definition:** Endpoint = IP + port.

**API:** socket, bind, listen, accept, connect, send, recv.

**Types:** SOCK_STREAM (TCP), SOCK_DGRAM (UDP).

**Recall:** *Program's network door.*

### QUIC

**Definition:** UDP-based transport; multiplexed streams.

**Features:** 0-RTT handshake, connection migration, head-of-line blocking avoided.

**HTTP/3:** Runs over QUIC.

**Recall:** *Modern transport.*

---

## 12.4 Application Layer

### DNS

**Definition:** Domain Name System; maps names to IPs.

**Hierarchy:** Root → TLD → authoritative.

**Records:**
| Type | Meaning |
|------|---------|
| A | IPv4 address |
| AAAA | IPv6 address |
| CNAME | Alias |
| MX | Mail server |
| NS | Name server |
| TXT | Text |

**Caching:** TTL-based.

**Recall:** *Internet phonebook.*

### HTTP/HTTPS

**HTTP:** Request/response.
- Methods: GET, POST, PUT, DELETE, PATCH
- Status: 2xx success, 3xx redirect, 4xx client error, 5xx server error

**HTTPS:** HTTP over TLS.

**Versions:** 1.1, 2 (multiplexing), 3 (QUIC).

**Recall:** *Web protocol.*

### TLS

**Handshake:**
1. Client hello (ciphers, random)
2. Server hello (cert, random)
3. Key exchange
4. Finished

**TLS 1.3:** Fewer round trips; forward secrecy.

**Certificates:** X.509; CA-signed.

**Recall:** *Secure HTTP.*

### Load Balancing

**L4:** Based on IP/port.
**L7:** Based on content.

**Algorithms:** Round robin, least connections, consistent hashing.

**Tools:** Nginx, HAProxy, AWS ELB.

**Recall:** *Distribute traffic.*

### CDNs

**Definition:** Content Delivery Network; edge caching.

**Benefits:** Reduce latency; offload origin.

**Examples:** Akamai, Cloudflare, Fastly.

**Recall:** *Content near users.*

### VPNs

**Definition:** Encrypted tunnel over public network.

**Protocols:** IPsec, WireGuard, OpenVPN.

**Recall:** *Private over public.*

---

## 12.5 Wireless & Security

### Wireless Networking

**WiFi:** 802.11; 2.4/5/6 GHz; CSMA/CA.

**Cellular:** 4G LTE, 5G NR.

**Bluetooth:** Short-range; piconet.

**Recall:** *Radio-based networking.*

### 5G Core

**Features:** Network slicing, MEC, service-based architecture.

**Components:** AMF, SMF, UPF, NRF.

**Recall:** *Next-gen mobile core.*

### Delay-Tolerant Networking (DTN)

**Definition:** Store-and-forward for intermittent connectivity.

**Use:** Interplanetary, rural.

**Recall:** *Store and forward.*

### Network Security

**IDS/IPS:** Intrusion detection/prevention.

**DDoS mitigation:** Rate limiting, scrubbing.

**Zero trust:** Never trust, always verify.

**Recall:** *Protect the network.*

**SOTA:** QUIC, HTTP/3, eBPF networking, service meshes, satellite internet (Starlink), federated RL for multi-AS routing with privacy-preserving QoS optimization.

---
