# Nmap Network Reconnaissance

## Task 1 — Scan Local Network for Open Ports

### Objective

The objective of this task was to perform network reconnaissance on my local network using Nmap, identify active hosts and open TCP ports, determine the services associated with those ports, and understand the potential security exposure.

### Tools Used

- Nmap 7.99
- Wireshark
- Kali Linux
- VirtualBox

### Network Information

- Local IP: `10.75.204.71`
- Network: `10.75.204.0/24`
- Default Gateway: `10.75.204.39`
- Interface: `eth0`

### Methodology

1. Identified the local IP address and network interface using `ip addr`.
2. Checked the routing table using `ip route`.
3. Performed a TCP SYN scan of the local `/24` network.
4. Saved the scan results to `nmap_scan.txt`.
5. Performed service/version detection using Nmap `-sV`.
6. Saved the service detection results to `service_detection.txt`.
7. Performed OS detection using Nmap `-O`.
8. Saved the OS detection results to `os_detection.txt`.
9. Captured TCP SYN/SYN-ACK traffic using Wireshark to understand the behavior of the SYN scan.

### Nmap Scan Command

```bash
nmap -sS 10.75.204.0/24

### Service Detection Command

```bash
nmap -sV 10.75.204.30 10.75.204.39
nmap -sV -O 10.75.204.30### OS Detection Command

```bash
nmap -sV -O 10.75.204.30
```

## Scan Results

The scan covered 256 IP addresses and identified 3 hosts as up.

| IP Address | Open TCP Ports | Detected Service |
|---|---|---|
| 10.75.204.30 | 135, 139, 445 | Microsoft Windows RPC, NetBIOS, Microsoft-DS |
| 10.75.204.39 | 53 | DNS / dnsmasq 2.51 |
| 10.75.204.71 | None detected | Kali Linux host |

### Host: 10.75.204.30

Detected services:

- `135/tcp` — Microsoft Windows RPC
- `139/tcp` — Microsoft Windows NetBIOS
- `445/tcp` — Microsoft-DS
- OS family detected as Microsoft Windows

Nmap could not determine the exact operating system with certainty. Its OS detection produced several Windows possibilities, so the specific Windows version was not treated as confirmed.

### Host: 10.75.204.39

Detected service:

- `53/tcp` — DNS
- Version detected: `dnsmasq 2.51`

### Host: 10.75.204.71

This is the Kali Linux system used for the scan.

Nmap reported no open ports among the default 1,000 TCP ports scanned.

## Security Considerations

### Port 135 — MSRPC

Microsoft RPC is used for communication between Windows services.

Potential security consideration:

- Unnecessary exposure of RPC services can increase the attack surface.
- Access should be restricted to systems or networks that require the service.

### Port 139 — NetBIOS

NetBIOS Session Service is associated with legacy Windows network communication.

Potential security consideration:

- Legacy network services can increase network exposure when they are not required.
- Access should be restricted where possible.

### Port 445 — Microsoft-DS / SMB

Port 445 is commonly associated with Windows SMB/network file-sharing functionality.

Potential security consideration:

- SMB exposure increases the attack surface.
- SMB services should be appropriately restricted, securely configured, and kept patched.

### Port 53 — DNS

Port 53 is associated with DNS.

Nmap detected `dnsmasq 2.51` on `10.75.204.39`.

Potential security consideration:

- DNS services should be appropriately configured and restricted.
- The detected software version should be reviewed against the organization's patch and vulnerability-management requirements.

> An open port does not automatically mean that the host is vulnerable. Further configuration and vulnerability assessment would be required to determine whether a specific vulnerability exists.

## Wireshark Analysis

Wireshark was used to observe the TCP packets generated during the Nmap SYN scan.

The capture showed:

```text
10.75.204.71 → 10.75.204.30
SYN
```

followed by:

```text
10.75.204.30 → 10.75.204.71
SYN, ACK
```

This demonstrates the TCP SYN/SYN-ACK exchange associated with an open TCP port during the scan.

### Wireshark Filter

```text
ip.addr == 10.75.204.30 && tcp.flags.syn == 1
```

## Files Included

- `nmap_scan.txt` — Raw Nmap network scan results
- `service_detection.txt` — Nmap service/version detection results
- `os_detection.txt` — Nmap OS detection results
- `screenshots/` — Wireshark analysis screenshots

## Key Learnings

Through this task, I learned:

- How to identify a local network range.
- How to perform a TCP SYN scan using Nmap.
- How to identify open, closed, and filtered ports.
- How to perform service and version detection.
- How to perform basic OS detection.
- How exposed services can contribute to network attack surface.
- How Wireshark can be used to observe TCP SYN and SYN-ACK packets.
- The importance of documenting reconnaissance findings accurately.

## Conclusion

This task provided practical experience with basic network reconnaissance and service exposure analysis. The results demonstrate how Nmap can be used to identify active hosts, open ports, and associated services within an authorized local network.nmap -sV -O 10.75.204.30
nmap -sV -O 10.75.204.30
