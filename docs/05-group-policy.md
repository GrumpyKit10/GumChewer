# 05 – Group Policy Implementation

## Objective

To implement and test Group Policy Objects (GPOs) for centralized configuration of Windows 11 clients.  

This phase demonstrates:

- User-based policy enforcement  
- Desktop customization via GPO  
- Restricting access to Control Panel to enforce administrative policy  
- Integration with shared resources (desktop wallpaper)

---

## Pre-Configuration Requirements

Before applying GPOs:

- Windows 11 clients joined to `lab.local` domain  
- File share `\\GumChewer\Wallpapers` accessible with Read permissions  
- Users placed in **UsersOU OU**  
- Domain Controller properly configured with DNS and AD services

Ensuring prerequisites were met avoids GPO application errors and ensures policy inheritance works as intended.

---

## GPO Creation and Configuration

### 1. Create GPO

1. Opened **Group Policy Management Console (GPMC)**  
2. Right-clicked **Group Policy Objects in lab.local** → **New GPO*  
3. Named the GPO: `Workstation Restrictions`  

---

### 2. Configure Desktop Wallpaper

1. Edited the GPO → **User Configuration → Policies → Administrative Templates → Desktop → Desktop → Desktop Wallpaper**  
2. Enabled the policy  
3. Set the wallpaper path to:  
   ```
   \\GumChewer\Wallpapers\festive_rooster.jpg
   ```  
4. Selected **Fill** for wallpaper style to ensure correct display on all client resolutions  

> This ensures all users in the OU receive a consistent desktop background.

---

### 3. Deny Access to Control Panel

1. Edited the same GPO → **User Configuration → Policies → Administrative Templates → Control Panel**  
2. Enabled **Prohibit access to Control Panel and PC settings**  

> This prevents standard users from changing system or desktop settings, enforcing administrative control.

---

## Policy Scope and Linking

- GPO linked directly to **UsersOU OU**  
- Applies to all users within the OU  
- No filtering applied — all members of the OU receive the policy  
- Clients must be domain-joined and within the OU to receive the GPO

---

## Testing and Validation

On client machines:

1. Logged in as a domain user (e.g., `LAB\jdoe`)  
2. Updated policies manually:  
   ```
   gpupdate /force
   ```  
3. Verified desktop wallpaper changed to `festive_rooster.jpg`  
4. Attempted to open **Control Panel** → access denied  
5. Confirmed no errors in **Event Viewer → System / Application** related to policy application  

Successful testing confirmed:

- Correct wallpaper applied  
- Control Panel access successfully blocked  
- GPO processed without replication or inheritance errors

---

## Security and Administrative Considerations

- Centralized GPO deployment ensures uniform policy enforcement  
- Preventing Control Panel access reduces risk of user modifications to system settings  
- Using shared folder for wallpaper centralizes resource management and simplifies future updates  
- GPOs should always be tested on a non-administrative account to validate proper application

---

## Issues Encountered

### 1. Wallpaper Not Displaying

- **Cause:** Initial GPO applied before the share was accessible to all users  
- **Resolution:** Confirmed share permissions and refreshed policy with `gpupdate /force`  

### 2. Control Panel Access Not Blocked

- **Cause:** GPO linked to wrong OU or user not in target OU  
- **Resolution:** Verified user membership in **UsersOU OU** and relinked GPO  

---

## Lessons Learned

- GPOs must be linked to the correct OU to ensure intended application  
- Testing with actual domain users avoids surprises in enforcement  
- Centralized desktop management via GPO increases consistency and simplifies administrative overhead  
- Always confirm network access to shared resources referenced by policies (e.g., wallpapers)  
- Blocking Control Panel access is a simple but effective method to enforce user compliance in lab environments

---
