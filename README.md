# VLSM Subnetting & Static Routing — 192.168.5.0/24

A Cisco Packet Tracer VLSM lab using the `192.168.5.0/24` network.

## Lab Objectives

- Subnet `192.168.5.0/24` using VLSM.
- Provide enough addresses for four LANs and one point-to-point router link.
- Assign the **first usable address** to each PC.
- Assign the **last usable address** to each router LAN interface.
- Configure the point-to-point link.
- Configure static routes so all PCs can communicate.

## VLSM Addressing Plan

| Network | Requirement | Prefix | Network Address | First Usable | Last Usable | Broadcast |
|---|---:|---|---|---|---|---|
| LAN2 | 64 hosts | /25 | 192.168.5.0 | 192.168.5.1 | 192.168.5.126 | 192.168.5.127 |
| LAN1 | 45 hosts | /26 | 192.168.5.128 | 192.168.5.129 | 192.168.5.190 | 192.168.5.191 |
| LAN3 | 14 hosts | /28 | 192.168.5.192 | 192.168.5.193 | 192.168.5.206 | 192.168.5.207 |
| LAN4 | 9 hosts | /28 | 192.168.5.208 | 192.168.5.209 | 192.168.5.222 | 192.168.5.223 |
| R1-R2 | 2 hosts | /30 | 192.168.5.224 | 192.168.5.225 | 192.168.5.226 | 192.168.5.227 |

### Subnet Masks

- /25 = `255.255.255.128`
- /26 = `255.255.255.192`
- /28 = `255.255.255.240`
- /30 = `255.255.255.252`

## Device Addressing

| Device | Interface | IP Address | Mask | Purpose |
|---|---|---|---|---|
| PC-LAN2 | Fa0 | 192.168.5.1 | 255.255.255.128 | First usable |
| R1 | GigabitEthernet0/1 | 192.168.5.126 | 255.255.255.128 | Last usable |
| PC-LAN1 | Fa0 | 192.168.5.129 | 255.255.255.192 | First usable |
| R1 | GigabitEthernet0/0 | 192.168.5.190 | 255.255.255.192 | Last usable |
| R1 | GigabitEthernet0/0/0 | 192.168.5.225 | 255.255.255.252 | R1-R2 link |
| R2 | GigabitEthernet0/0/0 | 192.168.5.226 | 255.255.255.252 | R1-R2 link |
| PC-LAN3 | Fa0 | 192.168.5.193 | 255.255.255.240 | First usable |
| R2 | GigabitEthernet0/0 | 192.168.5.206 | 255.255.255.240 | Last usable |
| PC-LAN4 | Fa0 | 192.168.5.209 | 255.255.255.240 | First usable |
| R2 | GigabitEthernet0/1 | 192.168.5.222 | 255.255.255.240 | Last usable |

> Interface names follow the labels shown in the supplied topology. If your Packet Tracer router model uses different interface numbering, apply the same IP addressing to the corresponding physical interfaces.

## Default Gateways

- PC-LAN1 → `192.168.5.190`
- PC-LAN2 → `192.168.5.126`
- PC-LAN3 → `192.168.5.206`
- PC-LAN4 → `192.168.5.222`

## Static Routing

### R1

R1 is directly connected to LAN1, LAN2, and the R1-R2 link. It needs routes to LAN3 and LAN4:

```text
ip route 192.168.5.192 255.255.255.240 192.168.5.226
ip route 192.168.5.208 255.255.255.240 192.168.5.226
```

### R2

R2 is directly connected to LAN3, LAN4, and the R1-R2 link. It needs routes to LAN1 and LAN2:

```text
ip route 192.168.5.0 255.255.255.128 192.168.5.225
ip route 192.168.5.128 255.255.255.192 192.168.5.225
```

## Verification

On each router:

```text
show ip interface brief
show ip route
show running-config
```

Test connectivity:

```text
ping 192.168.5.226
```

From the PCs, test the remote LANs. For example, PC-LAN1 should be able to ping:

```text
192.168.5.1
192.168.5.193
192.168.5.209
```

## Repository Structure

```text
VLSM-192.168.5.0-24/
├── README.md
├── .gitignore
│
├── topology/
│   ├── VLSM_Topology.png
│   ├── VLSM_192.168.5.0_24.pkt
│   └── README.md
│
├── configs/
│   ├── R1-config.txt
│   └── R2-config.txt
│
└── docs/
    ├── addressing-table.md
    └── verification.md
```

### Packet Tracer Project

The `topology/` folder is intended to contain the completed Cisco Packet Tracer `.pkt` project.

**Important:** The supplied image is included as `VLSM_Topology.png`. The actual `.pkt` file must be saved from Cisco Packet Tracer after you build/configure the topology, then placed in the `topology/` folder.


## Skills Demonstrated

- IPv4 subnetting
- VLSM
- CIDR and subnet masks
- Network/broadcast/usable host calculation
- Cisco IOS interface configuration
- Static routing
- Default gateway configuration
- Basic network troubleshooting
- Packet Tracer topology implementation
