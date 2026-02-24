# 04 – File Share Permissions for Desktop Wallpaper Deployment

## Objective

Create a shared folder on the Domain Controller accessible to Windows 11 clients for centralized desktop wallpaper deployment via Group Policy.

This phase demonstrates:

- Share and NTFS permission management  
- Using built-in security principals (Authenticated Users) for controlled access  
- Preparing file resources for Group Policy deployment

---

## File Share Design Overview

The goal was to make a folder accessible to all domain-joined clients so a GPO could consistently apply a desktop background.

### Folder Location

```
C:\Wallpapers
```

This folder stores image(s) used as the desktop wallpaper for all client machines.

---

## Configuring Share and NTFS Permissions

### Share Permissions

1. Right-click `C:\Wallpapers` → **Properties** → **Sharing** tab → **Advanced Sharing**  
2. Checked **Share this folder**  
3. Clicked **Permissions**  
4. Added **Authenticated Users**  
5. Granted **Read** permission  

> Note: Read access is sufficient since clients only need to read the wallpaper file.

### NTFS Permissions

Under the **Security** tab:

| Principal           | Permission        |
|---------------------|-------------------|
| Authenticated Users | Read & Execute    |
| Administrators      | Full Control      |
| SYSTEM              | Full Control      |

Inheritance from parent folder was left enabled, but only these principals have explicit access.

---

## Permission Principles

- **Authenticated Users** covers all domain-joined accounts, appropriate for lab-based GPO deployment.  
- Access is read-only to prevent accidental modification or deletion.  
- Effective permission = the most restrictive between Share and NTFS permissions. In this case:
  - Share = Read  
  - NTFS = Read & Execute  
  - → Effective permission = Read

---

## Testing Access

On a client machine:

1. Opened `\\GumChewer\Wallpapers`  
2. Verified the wallpaper image could be read  
3. Confirmed no write or delete capability  

All domain-joined clients could access the shared folder as intended.

---

## Integration with Group Policy

The shared folder was used as the source path for desktop wallpaper deployment:

1. Opened **Group Policy Management Console (GPMC)**  
2. Created or edited a GPO linked to the **UsersOU OU**  
3. Configured:  
   - **User Configuration → Policies → Administrative Templates → Desktop → Desktop → Desktop Wallpaper**  
   - Set path to `\\GumChewer\Wallpapers\festive_rooster.jpg`  
   - Enabled the policy  
4. Linked the GPO to the **UsersOU OU**  

Clients applied the wallpaper after `gpupdate /force` or during the next login.

---

## Security Considerations

- Using **Authenticated Users** is acceptable for lab environments but should be avoided in production environments where stricter access control is needed.  
- Read-only access ensures clients cannot modify folder contents.  
- Centralized shares simplify administration and reduce redundancy.

---

## Issues Encountered

### 1. Access Denied on Client

- **Cause:** GPO initially linked to Workstations OU instead of UsersOU OU.  
- **Resolution:** Corrected GPO link to **UsersOU OU**; clients then accessed the wallpaper without issue.

---

## Lessons Learned

- Share and NTFS permissions must be aligned; Read access is required on both layers for GPO deployment.  
- Linking the correct OU is critical to ensure policies apply to the intended users.  
- Centralized file shares streamline GPO resource deployment and reduce administrative overhead.  
- Using Authenticated Users simplifies lab deployment but may not meet production security standards.