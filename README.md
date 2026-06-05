# Active Directory Home Lab

A virtualized cybersecurity home lab simulating a real-world enterprise Windows environment. Built to practice Active Directory administration, Group Policy hardening, network configuration, and offensive reconnaissance using Kali Linux.

---

## Tools & Technologies

| Category | Tools |
|----------|-------|
| Server OS | Windows Server 2022 |
| Attacker OS | Kali Linux |
| AD Management | Active Directory Users & Computers |
| Policy Management | Group Policy Management Editor |
| Offensive Tool | NetExec (nxc) |
| Protocol | SMB (Port 445) |

---

## Lab Architecture

- **Domain:** lab.local
- **Domain Controller:** DC01 — <DC-IP>
- **Subnet:** <SUBNET-MASK>
- **Default Gateway:** <GATEWAY-IP>
- **DNS:** <DNS-IP> (loopback for AD resolution)
- **Attacker Machine:** Kali Linux on the same subnet

---

## Active Directory Configuration

### IT_Admins Group Members

| Name | OU |
|------|----|
| Ada Lovelace | lab.local/HQ |
| Grace Hopper | lab.local/HQ |
| John Smith | lab.local/HQ |
| Linus Torvalds | lab.local/HQ |

---

## Group Policy Hardening

Applied via Security_AccountLockout Policy under:
`Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Account Lockout Policy`

| Policy | Setting |
|--------|---------|
| Account lockout threshold | 5 invalid logon attempts |
| Account lockout duration | 30 minutes |
| Reset account lockout counter after | 30 minutes |

Policy was applied domain-wide using:

```
gpupdate /force
```

---

## Offensive Reconnaissance

Used NetExec (nxc) from Kali Linux to perform SMB credential validation against the domain controller, simulating attacker enumeration of domain accounts.

### Commands Run

```bash
nxc smb <DC-IP> -u jsmith -p Mofo1234
nxc smb <DC-IP> -u alovelace -p SecureAdmin2026!
nxc smb <DC-IP> -u ltorvalds -p SecureAdmin2026!
nxc smb <DC-IP> -u ghopper -p SecureAdmin2026!
```

All accounts successfully authenticated against DC01, confirming credential validity and SMB signing status.

---

## Screenshots

### SMB Credential Validation (Kali Linux)
![NetExec SMB scan](screenshots/nxc_smb.png)

### IT_Admins Group Members
![AD Group](screenshots/it_admins_group.png)

### Group Policy — Account Lockout Settings
![GPO Settings](screenshots/gpo_lockout.png)

### GPO Applied via gpupdate /force
![gpupdate](screenshots/gpupdate.png)

### Static IP Configuration on DC01
![IP Config](screenshots/ip_config.png)

---

## What I Learned

- How to promote a Windows Server to a Domain Controller and configure a domain from scratch
- How to create and manage Active Directory users, groups, and OUs
- How to write and enforce Group Policy Objects (GPOs) for account security
- How attackers use tools like NetExec to enumerate and validate credentials over SMB
- How to configure static IP and DNS settings for stable AD name resolution

---

## Author

Mofolorunsho Adeleke
GitHub: https://github.com/Mo200909
