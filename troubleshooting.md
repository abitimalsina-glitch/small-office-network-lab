# Troubleshooting & Verification

This document outlines the procedures and results used to verify the operational status of the Small Office Network, as well as the resolution of common configuration issues.

## Testing & Verification Procedures

The network's health was verified through a systematic approach:
1.  **Physical/Link Layer:** Checked for green lights on all switch and router ports.
2.  **Data Link Layer:** Verified VLAN assignments and Trunking protocols.
3.  **Network Layer:** Verified IP address assignment (DHCP) and Inter-VLAN routing (Pings).
4.  **Application Layer:** Verified DNS resolution (pinging by name if applicable).

## Common Troubleshooting Commands

The following Cisco IOS commands were essential for verifying the configuration:

*   `show ip interface brief`: To check the status and IP of all interfaces.
*   `show vlan brief`: To confirm VLAN creation and port mapping.
*   `show interfaces trunk`: To verify trunking status between switch and router.
*   `show ip route`: To inspect the routing table and ensure subnets are reachable.
*   `show ip dhcp binding`: To see current IP leases.
*   `ping [ip-address]`: To test basic connectivity.
*   `traceroute [ip-address]`: To identify the path taken by packets.

## Resolved Engineering Challenges

### Issue 1: Inter-VLAN Routing Failure
*   **Symptom:** Clients in VLAN 10 could not ping clients in VLAN 20.
*   **Root Cause:** The sub-interface on the router (`Gig0/0/0.20`) was missing the `encapsulation dot1Q 20` command.
*   **Resolution:** Applied the correct encapsulation command to the sub-interface.

### Issue 2: DHCP Request Timeout
*   **Symptom:** PCs were receiving APIPA addresses (169.254.x.x).
*   **Root Cause:** The switch port connecting to the DHCP server (router) was not configured as a trunk, or the native VLAN was mismatched.
*   **Resolution:** Verified `switchport mode trunk` on `Fa0/1` and ensured the router sub-interfaces were active.

## Proof of Connectivity

### Same-VLAN Ping Test

![Same VLAN Ping](../screenshots/same-vlan-ping-test.png)
*Figure 7: Success within the same broadcast domain.*

### Different-VLAN Ping Test (Inter-VLAN)

![Different VLAN Ping](../screenshots/different-vlan-ping-test.png)
*Figure 8: Successful communication between different departments via the router.*

### Client IP Configuration
Verification of dynamic IP assignment.
![IP Config VLAN 30](../screenshots/ip-config-vlan30.png)
*Figure 9: Successful DHCP lease for a client in the Reception VLAN.*

### Wireless Client IP Configuration
![IP Config VLAN 50](../screenshots/ip-config-vlan50.png)
*Figure 10: Successful DHCP lease for a wireless client on the Guest network.*