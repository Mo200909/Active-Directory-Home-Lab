# Active Directory Home Lab

A small home lab built to practice setting up and securing a Windows domain, then testing it like an attacker would.

## Setup

| What | Used |
|---|---|
| Server | Windows Server 2022 |
| Attacker machine | Kali Linux |
| Domain | lab.local |
| Domain controller | DC01 |

## What I built

**4 user accounts** in an OU called `HQ`:
- Ada Lovelace
- Grace Hopper
- John Smith
- Linus Torvalds

**A lockout policy** through Group Policy:
- 5 failed logins locks the account
- Lockout lasts 30 minutes
- Counter resets after 30 minutes

Path in GPO: Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Account Lockout Policy

Applied it domain-wide with `gpupdate /force`.

## Testing it like an attacker

Used NetExec (nxc) from Kali to check if the accounts could authenticate over SMB — basically simulating how an attacker would try to validate stolen or guessed credentials.

```
nxc smb <DC-IP> -u jsmith -p [password]
nxc smb <DC-IP> -u alovelace -p [password]
nxc smb <DC-IP> -u ltorvalds -p [password]
nxc smb <DC-IP> -u ghopper -p [password]
```

All 4 accounts authenticated successfully, and SMB signing came back enabled — meaning the domain controller is protected against relay attacks.

## What I learned

- How to stand up a domain controller from scratch
- How to create and organize AD users, groups, and OUs
- How to write and enforce a GPO
- How attackers use tools like NetExec to test stolen credentials
- How to set up static IP/DNS so AD name resolution actually works

## Author

Mofolorunsho Adeleke
GitHub: https://github.com/Mo200909
