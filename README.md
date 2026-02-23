# GumChewer – Active Directory Home Lab

## Overview

This project documents the design and implementation of a Windows Server 2022 Active Directory lab built in VirtualBox. The lab includes one domain controller and two Windows 11 domain-joined clients. It was created to practice:

- Active Directory Domain Services (AD DS)
- DNS configuration
- OU structure and user management
- Group Policy deployment
- NTFS and share permission management
- Troubleshooting domain join and DNS issues

## Environment

### Host Specs
- OS: Microsoft Windows 11 Home (x64)
- CPU: AMD Ryzen 7 3700X
- Motherboard: MSI B450 TOMAHAWK MAX (MS-7C02)
- Memory: 2 x 16GB DDR4 SDRAM
- GPU: AMD Radeon RX5700 XT
- Storage: 2TB SATA HDD & 1TB NVMe SSD

### Hypervisor
- Oracle VM VirtualBox

### Virtual Machine Configuration

#### Domain Controller
- OS: Windows Server 2022 (Desktop Experience)
- RAM: X GB
- CPU: X cores
- Disk: X GB (VDI, dynamically allocated)
- Network: Internal Network (LABNET)
- Static IP: 192.168.100.10

#### Client 01
- OS: Windows 11 Pro
- RAM: X GB
- CPU: X cores
- Disk: X GB
- Network: Internal Network (LABNET)
- Static IP: 192.168.100.20
- DNS: 192.168.100.10

#### Client 02
(Same structure)