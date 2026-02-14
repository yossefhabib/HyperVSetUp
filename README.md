<h1>Hyper-V Kali Deployment (Windows 11 Pro)</h1>

## Overview 
This project documents setting up virtual machine lab environment using Microsoft Hyper-V on Windows 11 Pro and a preconfigured Kali Linux VHDX image.

## The objective was to: 

- Enable and validate Hyper-V
- Configure proper network segmentation
- Verify image integrity (SHA256)
- Deploy Kali Linux securely
- Maintain isolation from the home network
- Establish a scalable foundation for future AD / SOC simulations
  

## Skills Demonstrated
-  Hypervisor configuration
-  Network segmentation design
-  Virtual infrastructure planning
-  Linux deployment
-  Hash integrity verification
-  Secure boot troubleshooting 
-  Resource allocation planning
-  Structured lab documentation
  
<h2>Host System Specifications</h2>

- OS: Windows 11 Pro
- RAM: 32GB
- Storage: Samsung 990 Pro NVMe 1TB
- RAM: 32GB DDR5-6000 (EXPO Enabled)
- Hypervisor: Microsoft Hyper-V



## Step 1: Enable Hyper-V
1.  Control Panel → Programs → Turn Windows Features On or Off → Hyper-V (all components) <br />

<img width="571" height="390" alt="1) Enabling Hyper-V" src="https://github.com/user-attachments/assets/f88914d4-f72d-4df1-afb9-d78617173bec" />
<br />
<br />

2.  Restarted device and checked status of Hyper-V through Windows shell (admin) command:
         <br />   get-WindowsOptionalFeature -online -FeatureName Microsoft-Hyper-V-All <br />
<br /> 
<img width="1065" height="326" alt="2) Checking status of hyperv" src="https://github.com/user-attachments/assets/0b204a14-acec-44a6-8701-1e535be94221" />

## Step 2: Network Architecture Configuration

Proper network segmentation is critical for cybersecurity lab safety.

### Created Virtual Switches:
1.  Internal Switch:
  -  Used for isolated lab environments
  -  Prevents attack traffic from reaching home LAN
  -  Ideal for AD, lateral movement, and malware simulation

<br />

2.  External Switch:
  -  Used for controlled internet access
  -  Bridges to physical NIC
  -  Allows updates when required

<br />

<img width="718" height="684" alt="5) external network switch for labs " src="https://github.com/user-attachments/assets/363ae15f-8274-435f-9fec-3e1c0968b510" />


## Step 3: Download & Verify Authentic Kali Linux Image
### Downloaded:
-  Kali Linux preconfigured Hyper-V VHDX

### Validate File:
-  Verified SHA256 hash against official Kali download page with PowerShell command:  
  -  Get-FileHash .\kali-linux-2025.4-hyperv-amd64.vhdx
  -  Purpose: Ensures file integrity and prevents tampered image deployment.
<br />
<img width="1109" height="622" alt="6) Verifying SHA256 sum" src="https://github.com/user-attachments/assets/23650c3e-c542-4673-b547-ce0129ff90f3" />
<img width="346" height="354" alt="6) grabbing the SHA256 sum " src="https://github.com/user-attachments/assets/f4733a45-80db-4cc0-b6d4-c74e4b0accce" />

## Step 4: Lab Storage Organization
### Created dedicated lab directory in root:
Moved VHDX file from Downloads to structured storage location: C:\HyperV\Kali\

Reason:
-  Prevent accidental deletion
-  Maintain professional lab organization
-  Improve documentation clarity
-  Simplify future scaling

## Step 5: Virtual Machine Creation
Hyper-V Manager → New → Virtual Machine

<br />

Configuration:
-  Generation: 2 (UEFI-based firmware)
-  Startup RAM: 8192 MB
-  Dynamic Memory: Enabled
-  RAM: 8192 MB
-  Network Adapter: Internal Switch (isolated lab design)
-  Virtual Hard Disk: Use Existing VHDX
  
<br />

<img width="711" height="526" alt="7) Completed wizard " src="https://github.com/user-attachments/assets/43f29d12-941d-4379-8c5b-4015b8900c2e" />

<br />

## Step 6: Secure Boot Adjustment
### Since Kali Linux is a non-Windows OS:
-  VM → Settings → Security → Disable Secure Boot
<br />
Reason:
Prevents UEFI boot validation failure under Windows Secure Boot template.

## Step 7: First Boot
### Started VM via: Right-click VM → Connect → Start

Login credentials:
-  Username: kali
-  Password: kali
<br/>

✅Successful boot confirmed 

<img width="2067" height="1018" alt="and we are up and running " src="https://github.com/user-attachments/assets/de6f4109-fd25-4605-b843-b6b60df01983" />

Post-Deployment Validation

Inside Kali CLI:
-  ping 8.8.8.8
-  Verified network interface presence and connectivity (which should be unreachable since Internal).

  <br>

<img width="1019" height="866" alt="ping" src="https://github.com/user-attachments/assets/7ed36930-8260-4b54-97a4-e41dc19d0bb7" />

<br> 

## Architectural Decisions: 
| Decision                       | Rational                   
| ------------------------------ | -----------------------------------------------------| 
| **Internal Switch**            | Protect home network from attack traffic             | 
| **8GB RAM**                    | RAM	Supports offensive tools without starving host  | 
| **Kali Linux**                 | Go to OS for all cyber needs                         | 
| **NVMe storage**               | High I/O performance during scans                    | 
| **SHA256 verification**        | Supply chain integrity validation                    | 

## Security Considerations
-  Kali connected to Internal switch by default
-  External access used only when required
-  No attack traffic exposed to production/home LAN
-  Hash validation performed prior to deployment

</p>
