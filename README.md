<div align="center">

# 🔐 Cybersecurity Lab Setup & Configuration

### Building a dedicated virtual environment for ethical hacking and penetration-testing practice

**Vishal Pal** · **Networkwalks | Batch B084**

![Cybersecurity](https://img.shields.io/badge/Focus-Cybersecurity-20232A?style=for-the-badge&logo=kalilinux&logoColor=white)
![VirtualBox](https://img.shields.io/badge/VirtualBox-7.2-183A61?style=for-the-badge&logo=virtualbox&logoColor=white)
![Kali Linux](https://img.shields.io/badge/Kali%20Linux-2026.2-557C94?style=for-the-badge&logo=kalilinux&logoColor=white)
![Batch](https://img.shields.io/badge/Batch-B084-C00000?style=for-the-badge)

</div>

---

## 📖 About the Project

As part of my Cybersecurity program at **Networkwalks**, I completed this hands-on project to build a virtual lab for cybersecurity learning and practical exercises.

I used **Oracle VirtualBox** as the virtualization platform and **Kali Linux** as the security-focused operating system. I also configured a private NAT Network so the lab can be expanded with additional virtual machines for controlled testing.

The aim was not just to install the tools, but to understand how the virtual machine, network settings, and recovery options fit together in a practical lab environment.

## 🎯 What I Worked On

- Installed the required archive utility and VirtualBox.
- Imported and configured the Kali Linux virtual machine.
- Created a dedicated NAT Network in VirtualBox.
- Connected Kali Linux to the lab network.
- Configured and checked the VM's IPv4 network settings.
- Verified basic network and DNS connectivity.
- Created a clean VM snapshot as a recovery point.
- Recorded the setup, checks, and troubleshooting notes.

## 🧱 Lab Design

The lab uses a private virtual network. This provides a starting point for connecting more VMs later, such as intentionally vulnerable target machines, while keeping practice within an environment I control.

```text
                 Host Computer
                      |
                Oracle VirtualBox
                      |
                 NAT Network
                 10.0.0.0/24
                      |
                 Kali Linux VM
                   10.0.0.2
                      |
              Future lab machines
              (when added to the
                same NAT Network)
```

> The diagram represents the intended lab layout. Additional target machines can be connected as the lab grows.

## ⚙️ Configuration Summary

The following table records the network and VM configuration used in the reference lab setup. Hardware specifications can vary by host system.

| Component | Configuration |
|---|---|
| Hypervisor | Oracle VirtualBox 7.2 |
| Security operating system | Kali Linux 2026.2 |
| Virtual machine memory | 2048 MB |
| Virtual network type | NAT Network |
| Network name | `NatNetwork` |
| IPv4 network | `10.0.0.0/24` |
| Kali Linux address | `10.0.0.2/24` |
| Default gateway | `10.0.0.1` |
| DNS server | `8.8.8.8` |
| Example future VM range | `10.0.0.3` – `10.0.0.99` |

*Note: IP addresses and resource values are lab configuration details; they may need to be adjusted for a different setup.*

---

## 🛠️ Implementation Walkthrough

### 1. Prepare the required software

I installed **7-Zip** to extract the Kali Linux virtual-machine package when required. This prepared the downloaded VM files for import into VirtualBox.

### 2. Install VirtualBox

I installed Oracle VirtualBox to create and manage the virtual machines used in the lab.

### 3. Set up a dedicated NAT Network

Inside VirtualBox's network settings, I created a NAT Network with the following configuration:

```text
Network Name : NatNetwork
IPv4 Prefix  : 10.0.0.0/24
DHCP         : Enabled
IPv6         : Disabled
```

I chose a NAT Network because it can allow multiple VMs on the same virtual network to communicate with each other while also providing outbound network connectivity.

### 4. Import and configure Kali Linux

I imported the Kali Linux VM into VirtualBox and configured its network adapter to use the newly created NAT Network.

```text
Adapter       : Adapter 1
Attached to   : NAT Network
Network       : NatNetwork
Adapter Type  : Intel PRO/1000 MT Desktop
Memory        : 2048 MB
```

I also configured a shared folder to make file transfer between the host computer and the Kali VM more convenient.

### 5. Configure Kali Linux networking

I checked the network configuration in Kali Linux and set up the IPv4 details for the lab.

```text
IP Address    : 10.0.0.2
Subnet Mask   : 255.255.255.0
Gateway       : 10.0.0.1
DNS           : 8.8.8.8
```

Using a consistent address makes it easier to identify the Kali VM and refer to it during later lab exercises.

### 6. Save a clean VM snapshot

After completing the initial setup, I created a VirtualBox snapshot named:

```text
Clean Kali - Network Setup
```

This snapshot serves as a baseline that can be restored if later experiments change the VM or its configuration.

---

## 🔎 Verification Checklist

After configuration, I used the following checks to verify the lab setup.

| Check | Command | What it checks |
|---|---|---|
| View network interfaces | `ip a` | Confirms the assigned IP address |
| Test gateway | `ping 10.0.0.1` | Checks connectivity to the gateway |
| Test external connectivity | `ping 8.8.8.8` | Checks connectivity using an IP address |
| Test DNS resolution | `nslookup networkwalks.com` | Checks whether the domain resolves |
| Check Nmap installation | `nmap --version` | Displays the installed Nmap version |

### Expected network details

```text
Kali IP Address : 10.0.0.2/24
Gateway         : 10.0.0.1
DNS Server      : 8.8.8.8
```

The commands above are included as verification steps. Actual output can differ depending on the machine and network environment.

---

## 🐞 Troubleshooting Notes

Working through setup issues is part of learning how a virtual lab operates. These are the issues and resolution approaches documented for this setup.

### Issue 1: Connectivity after setting a static IP

Manual IPv4 configuration can sometimes affect connectivity, depending on the active Kali Linux network connection and NetworkManager settings.

One workaround documented for this setup was:

```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
```

The connection can then be restarted and connectivity tested again.

**Before using the command:** check the actual connection name on your VM. It may not be `Wired connection 1` on every installation.

### Issue 2: VirtualBox hardware-virtualization warning

A VM may fail to start if hardware virtualization is disabled in the computer's BIOS/UEFI settings.

The resolution steps are:

1. Restart the computer and enter BIOS/UEFI setup.
2. Locate the CPU or virtualization settings.
3. Enable Intel VT-x or the relevant hardware-virtualization option.
4. Save the changes and restart the computer.
5. Launch the Kali Linux VM again.

---

## 💡 Key Takeaways

Completing this lab helped me understand the building blocks of a practical cybersecurity environment.

- **Virtualization:** How a hypervisor runs a separate operating system inside a VM.
- **NAT vs. NAT Network:** Why a NAT Network is useful when multiple lab VMs need to communicate with each other.
- **Virtual networking:** How the VM's network adapter connects it to a selected virtual network.
- **IPv4 configuration:** The roles of an IP address, subnet mask, default gateway, and DNS server.
- **Snapshots:** Why it is useful to preserve a clean VM state before experimenting.
- **Verification and troubleshooting:** Why checking connectivity and documenting issues are important parts of setting up a reliable lab.

## 🔐 Responsible Use

This environment is intended for education, ethical hacking practice, and authorized security testing only.

I will use the lab and its tools only on systems that I own or have explicit permission to test. Any future scanning or testing exercises should remain within the authorized lab scope.

## 🔗 Tools & Learning Resources

- [7-Zip Downloads](https://www.7-zip.org/download.html)
- [Oracle VirtualBox Downloads](https://www.virtualbox.org/wiki/Downloads)
- [Kali Linux – Get Kali](https://www.kali.org/get-kali/)

---

## 👨‍💻 About Me

**Vishal Pal**  
Cybersecurity Trainee · Networkwalks · Batch B084

This project is part of my hands-on learning journey in cybersecurity. I am building practical foundations in Linux, networking, ethical hacking, vulnerability assessment, and penetration testing.

---

<div align="center">

**Week 01 · Cybersecurity & Pentesting Lab Setup**

*Learn by building. Verify by testing. Improve by documenting.*

</div>
