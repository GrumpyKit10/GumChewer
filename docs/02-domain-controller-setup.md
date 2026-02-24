# 02 – Domain Controller Setup

## Objective

To install and configure Active Directory Domain Services (AD DS) on the Windows Server 2022 virtual machine and promote it to a Domain Controller within a new forest.

This phase establishes centralized authentication, DNS resolution, and domain management capabilities for the lab environment.

---

## Pre-Configuration Requirements

Before promoting the server to a Domain Controller, the following configurations were completed:

- Server renamed to: **GumChewer**
- Static IP address configured: **192.168.100.10**
- Preferred DNS set to: **192.168.100.10**
- Verified internal network connectivity
- Confirmed system time accuracy

Static configuration was performed prior to promotion to prevent replication or DNS inconsistencies.

---

## Installing Active Directory Domain Services

1. Opened **Server Manager**
2. Selected **Add Roles and Features**
3. Role-based installation
4. Selected local server
5. Installed:
   - Active Directory Domain Services
   - DNS Server (auto-selected dependency)
6. Completed installation (no restart required at this stage)

---

## Promoting to Domain Controller

After AD DS role installation:

1. Selected **Promote this server to a domain controller**
2. Chose **Add a new forest**
3. Root domain name: `lab.local`
4. Forest Functional Level: Windows Server 2016 (default)
5. Domain Functional Level: Windows Server 2016 (default)
6. Enabled:
   - DNS Server
   - Global Catalog
7. Set Directory Services Restore Mode (DSRM) password
8. Reviewed NetBIOS name (auto-generated: LAB)
9. Completed prerequisite check
10. Confirmed installation

The server rebooted automatically after promotion.

---

## Post-Promotion Validation

After reboot, the following validation steps were performed:

- Logged in using domain credentials: `LAB\Administrator`
- Opened **Active Directory Users and Computers**
- Opened **DNS Manager**
- Verified Forward Lookup Zone for `lab.local`
- Verified SRV records were automatically created
- Confirmed server resolves domain name using:
  ```
  nslookup lab.local
  ```

Successful DNS resolution confirmed proper AD-integrated DNS functionality.

---

## Organizational Unit (OU) Structure

To avoid using default containers, the following Organizational Units were created:

- **UsersOU**
- **Workstations**
- **IT**

This structure allows:
- Clean Group Policy scoping
- Logical object separation
- Delegation of administrative permissions in future configurations

Default containers (Users, Computers) were avoided for production-style organization.

---

## Initial User Creation

Created the following test accounts within the **UsersOU** OU:

- jdoe
- asmith
- itadmin

Verified:
- User logon permissions
- Password policy enforcement
- Proper OU placement

---

## Configuration Decisions

### Why Static IP Was Required
Domain Controllers must use static addressing to ensure reliable DNS registration and client authentication.

### Why DNS Was Installed on the DC
Active Directory is tightly integrated with DNS. Hosting DNS on the Domain Controller ensures proper service record registration and domain resolution.

### Why Default Containers Were Avoided
Objects placed in default containers cannot have Group Policy Objects directly linked to them. Using OUs allows granular control and scalable policy design.

---

## Issues Encountered

### 1. Domain Controller Rename Attempt
- **Cause:** Attempted rename after AD DS promotion.
- **Impact:** Risk of metadata inconsistency.
- **Resolution:** Rebuilt VM and ensured final hostname was configured before promotion.

---

## Lessons Learned

- Active Directory is heavily dependent on correct DNS configuration.
- Domain Controllers should be fully configured (hostname, IP, networking) before promotion.
- OU design should be planned early to support scalable policy management.
- Proper validation (nslookup, DNS zone inspection, SRV record verification) prevents downstream authentication issues.

---
