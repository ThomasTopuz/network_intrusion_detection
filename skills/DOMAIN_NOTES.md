# Domain notes: UNSW-NB15 Normal versus DoS

These notes support the data loader and the planned exploration notebook. They document all categorical values observed in the supplied **Normal/DoS subset**: 133 protocol labels, 13 service labels and 10 state labels. Counts of distinct values are across both original partitions; they are not claims about every possible deployment category.

## Packets per flow: useful summary, not a collective feature

Each row is an observed flow record that aggregates packets. `spkts` is the number of source-direction packets and `dpkts` is the number in the destination direction.

`packets_per_flow = spkts + dpkts`

If a row has 20 source packets and 5 destination packets, the result is 25 packets in that one flow record. It is not 25 connections, 25 requests, or the total packets reaching a victim across the network. A logical connection can also be split across exporter records; avoid treating every row as a guaranteed complete application session.

The sum can make a plot easier to read, but adds no information that was absent from its inputs. It also discards direction: 100/0 and 50/50 both sum to 100, despite very different response patterns. Duration matters too: packet volume and packet intensity are different properties. Verify exporter rate definitions before assuming an exact relationship between `rate`, duration and packet counts.

For exploration, keep `spkts` and `dpkts` primary. Treat the sum as an optional descriptive view, named **packets per flow**, rather than automatically adding it to the final feature set. Response fraction and directional imbalance may expose more useful relationships, but are also deterministic transformations and need zero-denominator handling. None of these transformations creates cross-flow/collective evidence.

A packet-volume or one-sided-flow observation can be consistent with flooding, legitimate bulk transfer, one-way protocols or capture/export limitations. The available data alone do not measure server exhaustion or service availability. The selected 13 features contain neither timestamps nor destination identifiers for aggregating many flows into a victim/time-window flood.

## How to interpret the categorical fields

- **`proto` describes the packet/flow protocol label.** It includes transport, control, routing, encapsulation and historical identifiers, not just TCP and UDP. Categories are nominal; arbitrary numeric encoding would introduce artificial order.
- **`service` describes the supplied application/service annotation.** It is not an independently verified application identity. `-` is a meaningful unidentified-service placeholder, not the string zero and not proof that a flow has no application.
- **`state` describes the exporter's protocol-dependent transaction observation.** TCP termination, connectionless exchange status and ICMP message types coexist in this one field. Interpret state jointly with protocol; not every state is a TCP connection lifecycle step.

Do not equate a category appearing only among benchmark attacks with an inherently malicious protocol. The UNSW dataset was generated in a laboratory with synthetic attack behavior. Context/category shortcuts may reflect that collection process rather than transferable DoS behavior.

## Sources and interpretation limits

- [UNSW dataset description](https://research.unsw.edu.au/projects/unsw-nb15-dataset): dataset generation and Argus/Bro feature extraction.
- [Argus ra manual](https://openargus.org/oldsite/man/man1/ra.1.pdf), physical page 16: transaction and ICMP state explanations.
- [Argus protocol-name table](https://github.com/openargus/clients/blob/master/include/argus_util.h): exporter label aliases, including repeated/special labels.
- [Argus state and protocol formatting implementation](https://github.com/openargus/clients/blob/master/common/argus_util.c): FIN/RST status, protocol-dependent state and unnamed-protocol fallback.
- [IANA protocol-number registry](https://www.iana.org/assignments/protocol-numbers/): registered protocol names and descriptions.
- [RFC 1700 historical assigned numbers](https://www.rfc-editor.org/rfc/rfc1700.html): historical naming context. It is not the current registry.
- [IANA service-name registry](https://www.iana.org/assignments/service-names-port-numbers/): service-name reference; the dataset's service tags are not asserted to be port-verifiable from the selected columns.

The numeric protocol references below are **interpretations of the retrieved exporter name table**, not recovered raw numeric values from the CSV. The exact exporter version/configuration used for each dataset row has not been established. Some historical aliases differ from modern registry names. In particular, `any` can merge multiple identifiers; `unas` is an exporter fallback; `ip`, `ib`, `sep`, `mhrp` and `aes-sp3-d` require the listed caveats. Preserve their original values. The literal state `no` remains unresolved.

The machine-readable companion is [categorical_value_dictionary.csv](../categorical_value_dictionary.csv), keyed by `(feature, value)`, with meanings, caveats, sources and interpretation status.


## Service values

| Value | Meaning | Interpretation note |
|---|---|---|
| `-` | No identified/provided service annotation. | Treat as an explicit unidentified-service category; it does not prove no application or malicious behavior. |
| `dhcp` | Dynamic Host Configuration Protocol: assigns hosts network configuration such as IP addresses. | Configuration traffic has different expectations from web or bulk-transfer traffic. |
| `dns` | Domain Name System: resolves domain names and other DNS records. | Small requests and larger responses can be legitimate; direction and query type matter. |
| `ftp` | File Transfer Protocol control channel: commands, login and transfer negotiation. | Separate from the file-content data channel. |
| `ftp-data` | File Transfer Protocol data channel: carries file contents. | Large byte counts can represent a legitimate transfer. |
| `http` | Hypertext Transfer Protocol: web requests and responses. | A response can be much larger than its request; volume alone is not a DoS label. |
| `irc` | Internet Relay Chat: client/server messaging. | A service label identifies communication type, not whether its use is legitimate. |
| `pop3` | Post Office Protocol version 3: retrieves email from a server. | Message retrieval can create asymmetric byte volumes. |
| `radius` | Remote Authentication Dial-In User Service: network authentication, authorization and accounting. | Often infrastructure access-control traffic; do not infer user intent from the service name. |
| `smtp` | Simple Mail Transfer Protocol: sends and relays email. | Payload sizes vary with message content and attachments. |
| `snmp` | Simple Network Management Protocol: device monitoring and management. | Queries and notifications have different direction/response expectations. |
| `ssh` | Secure Shell: encrypted remote administration and related secure transfers. | Encryption and long sessions can be legitimate; this tag alone is not evidence of an attack. |
| `ssl` | Exporter service label for SSL/TLS-associated encrypted communication. | A historical category name; it does not identify the negotiated version or prove obsolete SSL is in use. |

## State values

| Value | Meaning | Interpretation note |
|---|---|---|
| `ACC` | Accepted/request answered: for TCP, a setup response has been seen; for applicable non-TCP traffic, a request/response exchange may be recorded. | Does not by itself prove a completed TCP handshake or successful application session. |
| `CON` | Connected/continuing transaction: protocol-dependent evidence of ongoing or bidirectional activity. | For connectionless traffic, do not read this as a TCP-style connection. |
| `ECO` | ICMP Echo Request, commonly associated with ping. | An ICMP message type rather than a TCP connection state. |
| `FIN` | TCP finish/termination observed (FIN-related status). | Not proof of benign traffic or a complete clean four-way close; interpret with packet directions and other evidence. |
| `INT` | Initial transaction status; often used for connectionless traffic or flows without evidence of an established exchange. | Not a universal synonym for failed TCP handshake, and not intrinsically anomalous. |
| `PAR` | ICMP Parameter Problem: a problem was reported in an IP header/parameter. | An ICMP error classification rather than partial TCP connection. |
| `REQ` | Requested/initial request status: connection request for TCP, with protocol-dependent request meaning elsewhere. | Not evidence by itself that the request was malicious or permanently unanswered. |
| `RST` | TCP reset observed: a connection was reset/refused/aborted. | Resets also occur legitimately; this alone does not prove a DoS attack. |
| `URN` | ICMP destination network unreachable. | Reachability error; not a generic label for every unreachable condition. |
| `no` | Unresolved literal exporter/dataset state token. | One Normal ICMP row is present in the supplied filtered data. Its precise meaning is not established by the retrieved documentation; preserve it and do not invent an expansion. |

## Protocol values

| Value | Meaning | Interpretation note |
|---|---|---|
| `3pc` | Third Party Connect Protocol | Protocol identity is context, not an intrinsic normal/attack decision. |
| `a/n` | Active Networks: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `aes-sp3-d` | Historical AES Security Protocol 3-D label at number 96; the current registry uses SCC-SP (Semaphore Communications Security Protocol). | This is an old protocol name, not proof of contemporary AES-encrypted traffic. |
| `any` | Exporter label shared by numbers 61, 63 and 68: host-internal, local-network or distributed-file-system protocol allocations. | The textual category merges several numeric identifiers; it is not a wildcard for arbitrary normal traffic. |
| `argus` | Historical registered IP protocol ARGUS (number 13). Do not infer that this flow is an Argus monitoring-tool record. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `aris` | ARIS: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `arp` | Address Resolution Protocol: maps local IPv4 addresses to link-layer addresses; this is a link-layer protocol, not an IP Protocol-field number. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `ax.25` | AX.25 Frames: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `bbn-rcc` | BBN RCC Monitoring | Protocol identity is context, not an intrinsic normal/attack decision. |
| `bna` | BNA: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `br-sat-mon` | Backroom SATNET Monitoring | Protocol identity is context, not an intrinsic normal/attack decision. |
| `cbt` | CBT: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `cftp` | CFTP: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `chaos` | Chaos: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `compaq-peer` | Compaq Peer Protocol | Protocol identity is context, not an intrinsic normal/attack decision. |
| `cphb` | Computer Protocol Heart Beat | Protocol identity is context, not an intrinsic normal/attack decision. |
| `cpnx` | Computer Protocol Network Executive | Protocol identity is context, not an intrinsic normal/attack decision. |
| `crtp` | Combat Radio Transport Protocol | Protocol identity is context, not an intrinsic normal/attack decision. |
| `crudp` | Combat Radio User Datagram | Protocol identity is context, not an intrinsic normal/attack decision. |
| `dcn` | DCN Measurement Subsystems | Protocol identity is context, not an intrinsic normal/attack decision. |
| `ddp` | Datagram Delivery Protocol | Protocol identity is context, not an intrinsic normal/attack decision. |
| `ddx` | D-II Data Exchange (DDX) | Protocol identity is context, not an intrinsic normal/attack decision. |
| `dgp` | Dissimilar Gateway Protocol | Protocol identity is context, not an intrinsic normal/attack decision. |
| `egp` | Exterior Gateway Protocol | Protocol identity is context, not an intrinsic normal/attack decision. |
| `eigrp` | Enhanced Interior Gateway Routing Protocol: a routing protocol, not end-user application traffic. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `emcon` | EMCON: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `encap` | Encapsulation Header: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `etherip` | Ethernet-within-IP encapsulation. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `fc` | Fibre Channel: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `fire` | FIRE: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `ggp` | Gateway-to-Gateway: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `gmtp` | GMTP: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `gre` | Generic Routing Encapsulation: carries another protocol inside an IP tunnel. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `hmp` | Host Monitoring: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `i-nlsp` | Integrated Net Layer Security TUBA | Protocol identity is context, not an intrinsic normal/attack decision. |
| `iatp` | Interactive Agent Transfer Protocol | Protocol identity is context, not an intrinsic normal/attack decision. |
| `ib` | Exporter name at number 253, a registry slot for experimentation/testing. | The retrieved mapping does not establish an InfiniBand interpretation; keep the exporter token and its uncertainty. |
| `icmp` | Internet Control Message Protocol: IP diagnostics and error reporting, including echo requests and unreachable messages. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `idpr` | Inter-Domain Policy Routing Protocol | Protocol identity is context, not an intrinsic normal/attack decision. |
| `idpr-cmtp` | IDPR Control Message Transport Proto | Protocol identity is context, not an intrinsic normal/attack decision. |
| `idrp` | Inter-Domain Routing Protocol | Protocol identity is context, not an intrinsic normal/attack decision. |
| `ifmp` | Ipsilon Flow Management Protocol | Protocol identity is context, not an intrinsic normal/attack decision. |
| `igmp` | Internet Group Management Protocol: manages IPv4 multicast group membership. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `igp` | any private interior gateway (used by Cisco for their IGRP) | Protocol identity is context, not an intrinsic normal/attack decision. |
| `il` | IL Transport Protocol | Protocol identity is context, not an intrinsic normal/attack decision. |
| `ip` | Legacy exporter label at IP number 0. The current registry assigns 0 to IPv6 Hop-by-Hop Options (HOPOPT); the text ip alone does not establish ordinary IPv4 traffic. | Historical exporter naming; packet-level interpretation requires original protocol numbers or capture. |
| `ipcomp` | IP Payload Compression Protocol: compresses IP payload data. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `ipcv` | Internet Packet Core Utility | Protocol identity is context, not an intrinsic normal/attack decision. |
| `ipip` | IP-within-IP encapsulation; a tunneling identifier distinct from the exporter label ipnip. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `iplt` | IPLT: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `ipnip` | Legacy exporter name for protocol number 4, currently named IPv4 encapsulation (IPv4 carried inside IP). | Protocol identity is context, not an intrinsic normal/attack decision. |
| `ippc` | Internet Pluribus Packet Core | Protocol identity is context, not an intrinsic normal/attack decision. |
| `ipv6` | IPv6 encapsulation: an IPv6 packet carried as the next protocol; this label alone does not describe all native IPv6 traffic. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `ipv6-frag` | An IPv6 fragmentation extension-header identifier. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `ipv6-no` | IPv6 No Next Header identifier: indicates no following next-header payload. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `ipv6-opts` | IPv6 destination-options extension-header identifier. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `ipv6-route` | An IPv6 routing extension-header identifier. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `ipx-n-ip` | IPX in IP | Protocol identity is context, not an intrinsic normal/attack decision. |
| `irtp` | Internet Reliable Transaction | Protocol identity is context, not an intrinsic normal/attack decision. |
| `isis` | ISIS over IPv4 | Protocol identity is context, not an intrinsic normal/attack decision. |
| `iso-ip` | ISO Internet Protocol | Protocol identity is context, not an intrinsic normal/attack decision. |
| `iso-tp4` | ISO Transport Protocol Class 4 | Protocol identity is context, not an intrinsic normal/attack decision. |
| `kryptolan` | Kryptolan: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `l2tp` | Layer Two Tunneling Protocol: transports layer-two traffic through a tunnel. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `larp` | Locus Address Resolution Protocol | Protocol identity is context, not an intrinsic normal/attack decision. |
| `leaf-1` | Leaf-1: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `leaf-2` | Leaf-2: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `merit-inp` | MERIT Internodal Protocol | Protocol identity is context, not an intrinsic normal/attack decision. |
| `mfe-nsp` | MFE Network Services Protocol | Protocol identity is context, not an intrinsic normal/attack decision. |
| `mhrp` | Historical Mobile Host Routing Protocol label at number 48; the current registry names this slot Dynamic Source Routing (DSR). | Historical naming differs from the current registry. |
| `micp` | Mobile Internetworking Control Pro. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `mobile` | Minimal IPv4 Encapsulation | Protocol identity is context, not an intrinsic normal/attack decision. |
| `mtp` | Multicast Transport Protocol | Protocol identity is context, not an intrinsic normal/attack decision. |
| `mux` | Multiplexing: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `narp` | NBMA Address Resolution Protocol | Protocol identity is context, not an intrinsic normal/attack decision. |
| `netblt` | Bulk Data Transfer Protocol | Protocol identity is context, not an intrinsic normal/attack decision. |
| `nsfnet-igp` | NSFNET-IGP: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `nvp` | Network Voice Protocol | Protocol identity is context, not an intrinsic normal/attack decision. |
| `ospf` | Open Shortest Path First: a routing protocol used by routers to exchange topology information. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `pgm` | PGM Reliable Transport Protocol | Protocol identity is context, not an intrinsic normal/attack decision. |
| `pim` | Protocol Independent Multicast: router communication for multicast routing. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `pipe` | Private IP Encapsulation within IP | Protocol identity is context, not an intrinsic normal/attack decision. |
| `pnni` | PNNI over IP | Protocol identity is context, not an intrinsic normal/attack decision. |
| `pri-enc` | any private encryption scheme | Protocol identity is context, not an intrinsic normal/attack decision. |
| `prm` | Packet Radio Measurement | Protocol identity is context, not an intrinsic normal/attack decision. |
| `ptp` | Performance Transparency Protocol. This label does not mean IEEE 1588 Precision Time Protocol. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `pup` | PUP: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `pvp` | Packet Video Protocol | Protocol identity is context, not an intrinsic normal/attack decision. |
| `qnx` | QNX: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `rdp` | Reliable Data Protocol. This IP protocol label does not mean Windows Remote Desktop Protocol. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `rsvp` | Reservation Protocol / RSVP-E2E-IGNORE | Protocol identity is context, not an intrinsic normal/attack decision. |
| `rtp` | Real-time Transport Protocol: commonly carries real-time media. This observed label is outside the retrieved IP-number name table; no numeric IP mapping is asserted. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `rvd` | MIT Remote Virtual Disk Protocol | Protocol identity is context, not an intrinsic normal/attack decision. |
| `sat-expak` | SATNET and Backroom EXPAK | Protocol identity is context, not an intrinsic normal/attack decision. |
| `sat-mon` | SATNET Monitoring: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `sccopmce` | SSCOPMCE: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `scps` | SCPS: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `sctp` | Stream Control Transmission Protocol: message-oriented transport supporting associations and multiple streams. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `sdrp` | Source Demand Routing Protocol | Protocol identity is context, not an intrinsic normal/attack decision. |
| `secure-vmtp` | SECURE-VMTP: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `sep` | Historical Sequential Exchange Protocol label at number 33; the current registry names number 33 DCCP. The legacy label is preserved rather than silently relabeled. | Historical label; do not assume the observed record is modern DCCP. |
| `skip` | SKIP: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `sm` | Simple Multicast Protocol | Protocol identity is context, not an intrinsic normal/attack decision. |
| `smp` | Simple Message Protocol | Protocol identity is context, not an intrinsic normal/attack decision. |
| `snp` | Sitara Networks Protocol | Protocol identity is context, not an intrinsic normal/attack decision. |
| `sprite-rpc` | Sprite RPC Protocol | Protocol identity is context, not an intrinsic normal/attack decision. |
| `sps` | Secure Packet Shield | Protocol identity is context, not an intrinsic normal/attack decision. |
| `srp` | SpectraLink Radio Protocol | Protocol identity is context, not an intrinsic normal/attack decision. |
| `st2` | Stream: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `stp` | Schedule Transfer Protocol. This IP protocol label does not mean Ethernet Spanning Tree Protocol. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `sun-nd` | SUN ND PROTOCOL-Temporary | Protocol identity is context, not an intrinsic normal/attack decision. |
| `swipe` | IP with Encryption | Protocol identity is context, not an intrinsic normal/attack decision. |
| `tcf` | TCF: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `tcp` | Transmission Control Protocol: reliable, connection-oriented transport; acknowledgments, setup and termination matter. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `tlsp` | Transport Layer Security Protocol using Kryptonet key management | Protocol identity is context, not an intrinsic normal/attack decision. |
| `tp++` | TP++ Transport Protocol | Protocol identity is context, not an intrinsic normal/attack decision. |
| `trunk-1` | Trunk-1: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `trunk-2` | Trunk-2: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `ttp` | Internet Protocol Traffic Manager | Protocol identity is context, not an intrinsic normal/attack decision. |
| `udp` | User Datagram Protocol: datagrams without a TCP-style connection handshake; legitimate request/response and one-way traffic both occur. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `unas` | Exporter fallback when a numeric IP protocol has no named entry in its table (unassigned/unnamed to that exporter). | Not a known application protocol and not proof of an attack; several numeric values can collapse into this label. |
| `uti` | UTI: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `vines` | VINES: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `visa` | VISA Protocol: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `vmtp` | VMTP: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `vrrp` | Virtual Router Redundancy Protocol: coordinates redundant routers serving a shared virtual-router role. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `wb-expak` | WIDEBAND EXPAK: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `wb-mon` | WIDEBAND Monitoring: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `wsn` | Wang Span Network | Protocol identity is context, not an intrinsic normal/attack decision. |
| `xnet` | Cross Net Debugger | Protocol identity is context, not an intrinsic normal/attack decision. |
| `xns-idp` | XEROX NS IDP | Protocol identity is context, not an intrinsic normal/attack decision. |
| `xtp` | XTP: historical/special-purpose registered protocol; the registry supplies little descriptive detail. | Protocol identity is context, not an intrinsic normal/attack decision. |
| `zero` | Exporter name at number 114: allocation for any 0-hop protocol. | The word zero is a protocol category, not a numeric missing value or zero packets. |
