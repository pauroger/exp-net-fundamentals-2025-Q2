# Journal

Things I learned along the way:

1) Different networks should have different IPs.

- Point-to-point “backyard cable” is not scalable. A better long-term strategy
  is to use private address space reserved by [RFC1918](https://datatracker.ietf.org/doc/html/rfc1918),
  which is not routable on the public internet.
- I chose 10.42.0.0/16 — this gives up to 65,536 IPs, and since each home gets a
  /24 (256 IPs), this setup could support up to 256 homes.
- This approach is also nice because avoids conflicts with 99% of home routers
  (which default to 192.168.x.x)

Example:

```sh
# IP Plan – Neighbourhood Mesh

## Block: 10.42.0.0/16

| Home | Subnet | Purpose |
|------|--------|---------|
| Home A | 10.42.1.0/24 | Main LAN |
| Home B | 10.42.2.0/24 | Main LAN |
| Home C | 10.42.3.0/24 | Reserved |
| Point-to-Point | 10.42.255.0/30 | P2P Interconnect |
```

2) `/30` shared link is overly simplistic.

- A /30 subnet only supports 2 usable IPs, which is fine for a point-to-point
  test but doesn’t scale.
- Worse: if the router reboots or the white box dies, the stream breaks — it’s a
  single point of failure. We need something more resilient and future-proof.

3) Goodbye “white boxes”, hello High Availability (HA) edge pairs.

- Instead of a single white box, we use a HA router pair — for example, 2 × Raspberry Pi devices.
- If Pi-A dies, then thanks to VRRP (Virtual Router Redundancy Protocol), Pi-B
  advertises the VIP (virtual IP), and the stream keeps flowing.
- Bonus: we can hide this redundancy behind a single IP address for (potential) clients.

4) Introducing LACP: Link Aggregation for performance + failover

- We use LACP to bond multiple physical links between homes. A bond is a virtual
  network interface that combines multiple Ethernet cables into a single logical
  interface.

- Benefits:
  - More throughput: Two 1 Gb/s cables can carry 2 Gb/s total.
  - Resilience (failover): If one cable fails or is unplugged, the other keeps
    carrying traffic.
  - Simplified management: Devices treat the bond as a single IP and MAC
    address, so no reconfiguration is needed when something breaks.
