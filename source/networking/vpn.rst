VPN
===

This is a note from studying VPNs with OpenBSD.

Types of VPN
============

* IPSec: built into kernel, fastest
* OpenVPN: application level
* OPenSSH: application level

IPSec
=====

Two types of security protocols:

* Authentication Header (AH): protects IP header
* Encapsulating Security Payload (ESP): protects body of the data

Two modes of operation:

* transport mode (host-to-host): protects only the payload of the IP packet, host-to-host only.
* tunnel mode (host-to-network, network-to-network): encapsulates the entire IP packet into a new IP packet

Security Associations:

* Handles shared states between VPN endpoints, keys for encryption, the algorithm..etc
* IKEv2 creates the SA

IKEv2:

* Mutually authenticates the endpoints (IKE phase 1)
* Set up the encrypted channel (IKE phase 1)
* Negotiate the shared secret (IKE phase 2)

Original Sources:
*****************
* http://www.kernel-panic.it/openbsd/vpn/vpn2.html
