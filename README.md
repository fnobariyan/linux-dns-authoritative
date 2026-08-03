DNS Infrastructure with BIND9
Overview

This project demonstrates the implementation of a complete DNS infrastructure using BIND9 on Linux.

The environment consists of three DNS servers:

Primary (Master) DNS Server
Secondary (Slave) DNS Server
Forwarding DNS Server

The project also demonstrates:

Forward and Reverse DNS Zones
Secure Zone Transfers using TSIG Authentication
DNS Forwarding
Authoritative DNS Configuration
Recursive DNS for Client Queries
Network Topology
Server	IP Address	Role
Master DNS	192.168.56.102	Authoritative Primary DNS
Forwarder DNS	192.168.56.103	Recursive Forwarding DNS
Slave DNS	192.168.56.104	Authoritative Secondary DNS

Domain:

mcloud.local
Project Architecture
                    Clients
                       |
                       |
              +------------------+
              | Forward DNS      |
              | 192.168.56.103   |
              +------------------+
                    |
        -----------------------------
        |                           |
        |                           |
+-------------------+      +-------------------+
| Master DNS        |----->| Slave DNS         |
| 192.168.56.102    | AXFR | 192.168.56.104    |
+-------------------+ TSIG +-------------------+
Features
Authoritative Master DNS Server
Secondary DNS Server
Automatic Zone Transfer
TSIG Authentication
Forward DNS Zone
Reverse DNS Zone
Recursive Forwarding Server
Access Control Lists (ACL)
DNS Notification (NOTIFY)
Master DNS Server

IP Address

192.168.56.102
Responsibilities
Stores the original DNS zone files
Authoritative for mcloud.local
Maintains Forward Lookup Zone
Maintains Reverse Lookup Zone
Sends DNS NOTIFY messages
Allows secure zone transfer using TSIG
Configuration Highlights
Recursion disabled
ACL configured for trusted clients
TSIG key authentication
Automatic NOTIFY
Reverse lookup support
Slave DNS Server

IP Address

192.168.56.104
Responsibilities
Receives zone data from the Master
Provides redundancy
Answers authoritative queries
Synchronizes automatically after zone updates
Configuration Highlights
Slave Zone
TSIG authentication
Automatic synchronization
No recursion
Forwarding DNS Server

IP Address

192.168.56.103
Responsibilities
Receives client DNS requests
Performs recursive lookups
Forwards requests for mcloud.local to the authoritative DNS servers
Configuration Highlights
Recursive DNS enabled
Forward Only Mode
Uses both Master and Slave as forwarders
Security

Implemented security mechanisms include:

TSIG authentication using HMAC-SHA512
Restricted zone transfers
ACL-based query restrictions
Hidden BIND version
Disabled DNSSEC (Lab Environment)
DNS Zones

Forward Zone

mcloud.local

Reverse Zone

56.168.192.in-addr.arpa
Zone Transfer

The Master DNS server allows zone transfers only to the authorized Slave server using TSIG authentication.

Master
   |
   |  AXFR + TSIG
   |
Slave

This prevents unauthorized servers from downloading DNS zone data.

Useful Commands

Validate Configuration

named-checkconf

Validate Zone File

named-checkzone mcloud.local db.mcloud.local

Restart BIND

systemctl restart named

Enable Service

systemctl enable named

Check Service Status

systemctl status named
Testing

Forward Lookup

dig @192.168.56.102 server1.mcloud.local

Reverse Lookup

dig -x 192.168.56.X

Query Slave

dig @192.168.56.104 server1.mcloud.local

Query Forwarder

dig @192.168.56.103 server1.mcloud.local

Verify Zone Transfer

rndc reload
journalctl -u named
Technologies Used
BIND9
Linux
DNS
TSIG
AXFR
ACL
Forward DNS
Reverse DNS
Learning Outcomes

Through this project, I gained hands-on experience with:

Building an authoritative DNS infrastructure
Configuring Primary and Secondary DNS servers
Implementing secure zone transfers
Configuring recursive forwarding
Creating Forward and Reverse DNS zones
Applying DNS security best practices
Troubleshooting BIND9 configurations
