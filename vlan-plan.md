# VLAN Plan

This document outlines the logical segmentation of the Small Office Network. By implementing Virtual LANs (VLANs), we have separated the physical network into multiple distinct broadcast domains to enhance security, reduce congestion, and improve manageability.

## VLAN Table

| VLAN ID | VLAN Name | Subnet Associated | Assigned Ports | Gateway (SVI/Sub-interface) |
| :--- | :--- | :--- | :--- | :--- |
| 10 | ADMIN | 192.168.10.0/24 | Fa0/2 - Fa0/4 | 192.168.10.1 |
| 20 | FINANCE | 192.168.20.0/24 | Fa0/5 - Fa0/6 | 192.168.20.1 |
| 30 | RECEPTION | 192.168.30.0/24 | Fa0/7 - Fa0/8 | 192.168.30.1 |
| 40 | PRINTERS | 192.168.40.0/24 | Fa0/9 - Fa0/10 | 192.168.40.1 |
| 50 | WIRELESS | 192.168.50.0/24 | Fa0/11 | 192.168.50.1 |

## Design Rationale

The network was segmented based on departmental roles and device functions:
*   **Security & Isolation:** Sensitive departments like *Finance* and *Admin* are isolated from the *Reception* and *Wireless* networks. This prevents unauthorized access to internal resources.
*   **Broadcast Domain Reduction:** By limiting the size of each broadcast domain, we reduce unnecessary network traffic (e.g., ARP requests) and improve overall bandwidth efficiency.
*   **Quality of Service (QoS):** Segmentation allows for future implementation of QoS policies, such as prioritizing printer traffic or limiting bandwidth for the guest wireless network.

## Trunking & Native VLANs

To facilitate communication between the switch and the router (Inter-VLAN Routing), a trunk link is established:

*   **Interface:** `FastEthernet 0/1` on the Switch connected to `GigabitEthernet 0/0/0` on the Router.
*   **Encapsulation:** IEEE 802.1Q (Dot1Q).
*   **Allowed VLANs:** 10, 20, 30, 40, 50.
*   **Native VLAN:** VLAN 1 (Default).

The Router-on-a-Stick configuration utilizes sub-interfaces on the router's physical port, each corresponding to a specific VLAN tag.

![VLAN Brief](../screenshots/vlan-brief.png)
*Figure 11: Verification of VLAN assignments on the Access Switch.*
