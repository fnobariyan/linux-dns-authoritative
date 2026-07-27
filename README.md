# Authoritative DNS Server with BIND9

## Overview

This project demonstrates how to configure an **Authoritative DNS Server** using **BIND9** on Linux.

The DNS server is responsible for providing authoritative answers for the local domain:

```text
mcloud.local
```

The project includes zone creation, DNS records configuration, and validation using standard BIND utilities.

---

## Environment

| Component        | Value                    |
| ---------------- | ------------------------ |
| Operating System | Rocky Linux / RHEL 9     |
| DNS Software     | BIND9 (named)            |
| DNS Role         | Authoritative DNS Server |
| Domain           | mcloud.local             |
| Server Hostname  | ns1                      |
| DNS Server IP    | 192.168.56.102           |

---

## Network Configuration

| Interface | IP Address     | Purpose               |
| --------- | -------------- | --------------------- |
| enp0s3    | 192.168.56.102 | Host-Only Network     |
| enp0s8    | 10.0.2.15      | NAT (Internet Access) |

---

## DNS Architecture

```
                     Client
                        |
                        |
                +----------------+
                |  ns1           |
                |  BIND9         |
                | Authoritative  |
                +----------------+
                        |
          -------------------------------
          |             |              |
       ns1           www             lms
 192.168.56.102 192.168.56.160 192.168.56.170
```

---

## Zone Configuration

Zone Name

```
mcloud.local
```

Example records

| Record | Type | Address        |
| ------ | ---- | -------------- |
| @      | A    | 192.168.56.102 |
| ns1    | A    | 192.168.56.102 |
| www    | A    | 192.168.56.160 |
| lms    | A    | 192.168.56.170 |

Name Server

```
NS  ns1.mcloud.local.
```

---

## named.conf

The server is configured as a **Master (Primary) DNS Server**.

Important configuration:

```conf
recursion no;

zone "mcloud.local" {
    type master;
    file "/etc/named/zones/db.mcloud.local";
};
```

Since this server is authoritative, recursion is disabled.

---

## Directory Structure

```
/etc/named.conf

/etc/named.rfc1912.zones

/etc/named/zones/
└── db.mcloud.local
```

---

## Validation

Check zone syntax

```bash
named-checkzone mcloud.local /etc/named/zones/db.mcloud.local
```

Check BIND configuration

```bash
named-checkconf
```

Restart BIND

```bash
systemctl restart named
```

Enable service

```bash
systemctl enable named
```

Check status

```bash
systemctl status named
```

---

## Testing

Query the DNS server

```bash
dig @192.168.56.102 www.mcloud.local
```

or

```bash
nslookup www.mcloud.local 192.168.56.102
```

Expected Result

```
www.mcloud.local

192.168.56.160
```

---

## Project Objectives

* Deploy an Authoritative DNS Server using BIND9
* Configure Forward Lookup Zone
* Create A and NS records
* Validate BIND configuration
* Test DNS name resolution
* Practice Linux network services

---

## Skills Demonstrated

* Linux Administration
* BIND9 DNS Configuration
* DNS Zone Management
* Network Troubleshooting
* DNS Record Management
* Command Line Administration
* System Service Management (systemd)
