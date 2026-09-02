# Verification Checklist

## R1

```text
show ip interface brief
show ip route
```

Expected connected networks:

- 192.168.5.0/25
- 192.168.5.128/26
- 192.168.5.224/30

Expected static routes:

- 192.168.5.192/28 via 192.168.5.226
- 192.168.5.208/28 via 192.168.5.226

## R2

```text
show ip interface brief
show ip route
```

Expected connected networks:

- 192.168.5.192/28
- 192.168.5.208/28
- 192.168.5.224/30

Expected static routes:

- 192.168.5.0/25 via 192.168.5.225
- 192.168.5.128/26 via 192.168.5.225

## End-to-End Testing

Verify each PC has the correct IP address, subnet mask, and default gateway.

Recommended tests:

```text
PC-LAN1 → PC-LAN2
PC-LAN1 → PC-LAN3
PC-LAN1 → PC-LAN4
PC-LAN2 → PC-LAN3
PC-LAN2 → PC-LAN4
PC-LAN3 → PC-LAN4
```

All tests should succeed after the router interfaces, PC addressing, and static routes are configured correctly.
