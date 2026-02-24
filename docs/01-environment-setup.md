# 01 – Environment Setup

## Objective

To design and deploy a controlled virtual lab environment capable of supporting an Active Directory domain controller and multiple domain-joined clients for infrastructure practice.

The goal of this phase was to:

- Configure a stable virtualization platform  
- Deploy a Windows Server 2022 virtual machine  
- Deploy two Windows 11 client virtual machines  
- Establish isolated network connectivity between all systems  

---

## Host System Specifications

- **Operating System:** Windows 11 Home (x64)  
- **CPU:** AMD Ryzen 7 3700X  
- **Motherboard:** MSI B450 TOMAHAWK MAX (MS-7C02)  
- **Memory:** 32GB DDR4  
- **GPU:** AMD Radeon RX5700 XT  
- **Storage:** 2TB SATA HDD & 1TB NVMe SSD  

The NVMe SSD was used to store virtual machine disk files to improve performance.

---

## Hypervisor

- **Platform:** Oracle VM VirtualBox  
- Extension Pack installed (matching version)

VirtualBox was selected due to:
- No licensing cost  
- Support for multiple concurrent VMs  
- Internal networking capability  
- Snapshot functionality  

---

## Network Design

All virtual machines were configured on a VirtualBox **Internal Network** to simulate a private enterprise LAN.

- **Network Name:** adlabnet  
- **Subnet:** 192.168.100.0/24  
- **Default Gateway:** Not configured (intentionally isolated)

This design prevents interference from external DNS services and simulates a contained enterprise environment.

### IP Addressing Scheme

| System     | Role              | IP Address       | DNS              |
|------------|-------------------|------------------|------------------|
| GumChewer  | Domain Controller | 192.168.100.10   | 192.168.100.10   |
| Client01   | Client            | 192.168.100.20   | 192.168.100.10   |
| Client02   | Client            | 192.168.100.21   | 192.168.100.10   |

Static addressing was used to maintain predictable infrastructure behavior.

---

## Virtual Machine Configuration

### Domain Controller – GumChewer

- **Operating System:** Windows Server 2022 (Desktop Experience)
- **RAM:** 8 GB
- **CPU:** 4 vCPUs
- **Disk:** 80 GB (VDI, dynamically allocated)
- **Network Adapter:** Internal Network (adlabnet)
- **Firmware:** EFI Enabled
- **Guest Additions:** Installed

---

### Client 01 – WIN11-01

- **Operating System:** Windows 11 Pro
- **RAM:** 8 GB
- **CPU:** 4 vCPUs
- **Disk:** 65 GB
- **Network Adapter:** Internal Network (adlabnet)
- **Guest Additions:** Installed

---

### Client 02 – WIN11-02

Configuration mirrored WIN11-01 for consistency and redundancy testing.

---

## Installation Notes

### Windows Server 2022

- Installed using evaluation ISO
- Static IP configured prior to role installation
- Server renamed before domain promotion

### Windows 11

- Installed using local account (offline setup)
- Guest Additions installed after OS setup

---

## Validation

The environment was considered successfully deployed once:

- All VMs booted consistently
- Internal network connectivity was confirmed via ping tests
- Clients could resolve the server IP address
- No external network access was present (intentional isolation)

---

## Issues Encountered

### 1. Windows 11 Required Microsoft Account

- **Cause:** Network detected during OOBE setup  
- **Resolution:** Used offline setup method to create local account  

### 2. Windows Server Installed as CLI Only

- **Cause:** Unattended install installed CLI-only OS by default. 
- **Resolution:** Disabled unattended install and chose the correct OS when building the VM.  

---

## Lessons Learned

- Proper virtualization resource allocation significantly impacts VM performance.
- Isolated internal networking is ideal for AD labs to avoid external DNS interference.
- Static IP configuration and computer name should be configured before promoting a domain controller.
- Windows 11 installation requirements require adjustment in virtual lab environments.

