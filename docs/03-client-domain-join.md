# 03 – Client Domain Join

## Objective

To join a Windows 11 client virtual machine to the `lab.local` Active Directory domain and verify proper authentication, DNS resolution, and object placement within the domain structure.

This phase validates domain functionality from a client perspective and confirms that centralized authentication is operating correctly.

---

## Pre-Configuration Requirements

Before attempting the domain join, the following prerequisites were verified:

- Client VM installed and updated
- Connected to the same internal network as the Domain Controller
- Setup static IP address.
- DNS server manually configured to: **192.168.100.10**
- Verified connectivity to Domain Controller via:
  ```
  ping 192.168.100.10
  ```

### Why DNS Configuration Is Critical

Active Directory domain joins rely entirely on DNS to locate domain controllers through SRV records.

If a client uses external DNS (e.g., ISP or public DNS), domain join will fail because the domain controller cannot be discovered.

---

## Verifying DNS Resolution

Before joining the domain, DNS functionality was confirmed:

```
nslookup lab.local
```

Verified:
- Name resolution returned **192.168.100.10**
- DNS server listed as the Domain Controller

Additionally verified SRV record discovery:

```
nslookup
set type=SRV
_ldap._tcp.dc._msdcs.lab.local
```

Successful response confirmed that the client could locate domain services.

---

## Domain Join Procedure

1. Opened **System Properties**
2. Selected **Change settings**
3. Clicked **Change**
4. Selected **Domain**
5. Entered: `lab.local`
6. Provided domain credentials:
   - Username: `LAB\itadmin`
7. Received confirmation: *"Welcome to the lab.local domain"*
8. Restarted the client machine

---

## Post-Join Validation

After reboot:

- Logged in as: `LAB\jdoe`
- Confirmed domain authentication was successful
- Verified computer object created in Active Directory

### Verifying Computer Object Placement

By default, computer objects are placed in the **Computers** container.

The client computer object was:

- Located in **Computers**
- Moved to the **Workstations** OU for proper policy scoping

This ensures future Group Policy Objects can be applied appropriately.

---

## Group Policy Verification

To confirm domain policy application:

```
gpupdate /force
gpresult /r
```

Verified:
- Domain name displayed correctly
- Group Policy processing completed successfully
- No authentication errors present

---

## Security and Authentication Flow

When the client joined the domain:

1. The client queried DNS for domain controllers.
2. The Domain Controller authenticated credentials.
3. A secure computer account was created in Active Directory.
4. A trust relationship was established between the client and the domain.

This trust relationship enables:

- Centralized authentication
- Kerberos ticket granting
- Group Policy application
- Access to domain resources (file shares, printers, etc.)

---

## Issues Encountered

### 1. Initial Domain Join Failure

- **Cause:** Used incorrect credentials (LAB\Administrator)
- **Impact:** Domain join failed.
- **Resolution:** Used correct username when joining the domain:
  ```
  LAB\itadmin
  ```

After correction, domain join completed successfully.

---

## Lessons Learned

- Active Directory environments are entirely dependent on proper DNS configuration.
- Always verify SRV record resolution before attempting a domain join.
- Computer objects should be moved into appropriate OUs immediately after joining.
- Domain authentication establishes a secure trust relationship using Kerberos.

---
