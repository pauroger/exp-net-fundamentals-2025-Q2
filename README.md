# Networking Fundamentals Bootcamp

Repo Structure Overview

```text
README.md
projects/
└── week1/
    ├── cloud_networking/       – Journal plus screenshots on AWS, Azure and GCP networking.
    ├── diagramming/            – README and journal explaining network diagrams (with images).
    ├── env_automation/         – CloudFormation templates, a deploy script, and journal notes.
    ├── ip_address_management/  – Notes and ARM templates for VM deployments with IP settings.
    ├── linux_firewall_rules/   – Journal on configuring Linux firewall rules.
    ├── linux_networking/       – Journal on basic Linux networking (plus a diagram).
    ├── packet_tracer/          – Packet Tracer project file, journal, and screenshot.
    ├── windows_firewall_rules/ – Journal and HTML example for Windows firewall rules.
    └── windows_networking/     – Journal on Windows networking (with a screenshot).
└── week2/
    ├── cml_lab/                – Cisco Modeling Labs NAT lab configs.
    ├── cml_vpn/                – VPN lab with sample config.
    ├── cml_wireshark/          – Wireshark lab capturing traffic.
    ├── forward_proxy/          – VM-inception proxy notes lab.
    ├── ha_proxy/               – HAProxy load balancer with two backend servers.
    ├── load_balancer/          – Load balancer presentation notes.
    ├── nat_basics/             – NAT basics summary with diagrams.
    ├── reverse_proxy/          – Reverse proxy lab notes and images.
    └── zero-trust/             – Zero-trust presentation notes.
```

## "Stand-up" updates Week 2

2025-06-30

Deployed an HAProxy load balancer with two backend web servers using
Containerlab. Configured routing both in Azure and on my Windows box to enable
full network connectivity between the Windows VM, the Ubuntu host, and the
container network. See [Journal](projects/week2/ha_proxy/JOURNAL.md).

2025-06-23/25

I ran into some challenges with the VM setup, as I had to recreate everything
from scratch in Azure due to the AWS price scare. In the end, I successfully set
up an interesting VM-inception scenario:

- Forward Proxy Lab. See [Journal](projects/week2/forward_proxy/JOURNAL.md).
- Reverse Proxy Lab. See [Journal](projects/week2/reverse_proxy/JOURNAL.md).

2025-06-19

I noticed a surprising ~$45 charge from AWS, even though my EC2 instances were
turned off. It turns out that the networking components—like VPCs, subnets, NAT
gateways, and Elastic IPs—were still running silently in the background. I’ve
since shut down everything in AWS and instead spun up an Ubuntu box in Azure for
the Load Balancer lab. I have free credits there, and I find it easier to delete
all resources at once by removing the entire project.

2025-06-15/16

- Spent the last couple of days trying to get **Cisco CML** running via VMware,
  but hit compatibility issues on my Silicon Mac, even when using the Windows VM
  from last week! I’ve documented my progress and blockers in the Journal with
  screenshots. See [Journal](projects/week2/cml_lab/JOURNAL.md).
- **CML VPN Lab**. See [Journal](projects/week2/cml_vpn/JOURNAL.md).
- **CML Wireshark Lab**. See [Journal](projects/week2/cml_wireshark/JOURNAL.md).
- Summary of the **NAT Basics** presentation. See [Journal](projects/week2/nat_basics/assets/static_nat_example.png).

## "Stand-up" updates Week 1

2025-06-07/08/09/10

Followed the Week1 tutorials:

- **Cloud Networking**. See [Journal](projects/week1/cloud_networking/JOURNAL.md).
- **Linux Firewall Rules**. See [Journal](projects/week1/linux_firewall_rules/JOURNAL.md).
- **Linux Networking**. See [Journal](projects/week1/linux_networking/JOURNAL.md).
- **Windows Firewall Rules**. See [Journal](projects/week1/windows_firewall_rules/JOURNAL.md).
- **Cisco Packet Tracer**. See described diagram, packet flow, and more in
[Packet Tracer](#packet-tracer) section below.
- **Windows Networking**. See [Journal](projects/week1/windows_networking/JOURNAL.md).
- **IP Address Management**. Ran and tested myself every EC2 instance. I've also
created a Windows VM Machine in Azure. See [Journal](projects/week1/ip_address_management/JOURNAL.md).

2025-06-06

Template and shell script to create the cloud resources needed for the bootcamp
in AWS. Learned about the useful AWS commands that highlight what failed when
running the Cloud Formation (`aws cloudformation describe-change-set`).
See more in the [Cloud Formation](#cloud-resources-as-code) section blow.

2025-06-04

As I was answering the Readme questions on the diagram from the Livestream-Week
1 I got inspired into making a version 2. You can see the details in the
[Diagram](#diagram) section below.

## Diagram

![Technical Diagram](projects/week1/diagramming/assets/improved_diagram.png)

### [Diagram Readme](projects/week1/diagramming/README.md)

### [Diagram Journal](projects/week1/diagramming/JOURNAL.md)

## Cloud Resources as Code

![AWS Infra Composer](projects/week1/env_automation/assets/aws_infra_composer.png)

### [Cloud Resources Readme](projects/week1/env_automation/README.md)

### [Cloud Resources Journal](projects/week1/env_automation/JOURNAL.md)

## Packet Tracer

```sh
                 ┌──────────┐  Gi0/0
                 │ Router0  │  192.168.0.1/24   (default-gateway, DHCP)
                 │  ISR4331 │
                 └────┬─────┘
                      │
                      │ Gigabit-Ethernet
                      ▼
              ┌────────────────┐
              │ Switch0        │  Cisco Catalyst 2960-24TT
              └─┬────────┬─────┘
      Fa0/1 ▲   │        │   ▲ Fa0/2
            │   │        │   │
            │   │        │   │
          PC0   │        │  Server1
    DHCP client │        │  Static-IP ( 192.168.0.2/24 )
                │        │
    ────────────┘        └───────────────────────────────
```

### [Packet Tracer Journal](projects/week1/packet_tracer/JOURNAL.md)

## Presentation Summaries

- Load Balancers. See [Journal](projects/week2/load_balancer/JOURNAL.md).
- Zero-Trust. See [Journal](projects/week2/zero_trust/JOURNAL.md).
