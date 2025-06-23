# scanning
Learn to discover open ports on devices in your local network to understand network exposure.

# Local Network Port Scanning Guide

A hands-on guide to discovering open ports on your local network using Nmap. This project is designed for educational purposes to help you understand your network's exposure and basic security posture.

---


## 🎯 Objective

The goal of this exercise is to learn how to discover open ports on various devices (computers, phones, smart TVs, IoT devices, etc.) within your local network. By identifying these open ports, you can begin to understand the services running on your network and assess potential security risks.

---

## 🛠️ Tools Required

*   **Nmap (Network Mapper)**: An essential, free, and open-source tool for network discovery and security auditing.
    *   **Download**: (https://nmap.org/download.html)
*   **Wireshark (Optional)**: A powerful network protocol analyzer that lets you see what's happening on your network at a microscopic level.
    *   **Download**: (https://www.wireshark.org/download.html)

---

## 📝 Step-by-Step Guide

### Step 1: Install Nmap

Download and install Nmap for your operating system (Windows, macOS, or Linux) from the official website linked above. The installer will typically add Nmap to your system's PATH, allowing you to run it from any terminal or command prompt.

### Step 2: Find Your Local IP Range

Before you can scan, you need to know what to scan. You must identify the IP address range of your local network.

*   **On Windows:**
    Open Command Prompt (`cmd.exe`) and type:
    ```sh
    ipconfig
    ```
    Look for your "Wireless LAN adapter" or "Ethernet adapter". Note the `IPv4 Address` (e.g., `192.168.1.15`) and the `Subnet Mask` (e.g., `255.255.255.0`). A subnet mask of `255.255.255.0` means your network range is `192.168.1.0/24`.

*   **On macOS or Linux:**
    Open a terminal and type:
    ```sh
    ifconfig
    # or a more modern command:
    ip a
    ```
    Look for your active network interface (e.g., `en0` on Mac, `eth0` or `wlan0` on Linux). Find the `inet` address. If your IP is `192.168.1.15`, your network range is likely `192.168.1.0/24`. The `/24` is CIDR notation for the `255.255.255.0` subnet mask and means Nmap will scan all 256 addresses in that range (from `192.168.1.0` to `192.168.1.255`).

### Step 3: Run the Nmap Scan

Now, run the Nmap scan. We will use the `-sS` flag, which performs a TCP SYN scan (also known as a "half-open" or "stealth" scan). It's fast and less likely to be logged by target systems.

Open your terminal or command prompt and run the following command, **replacing `192.168.1.0/24` with your actual network range**.

```sh
nmap -sS 192.168.1.0/24
```
Nmap will now send packets to every potential host on your network and report back which hosts are up and which of their ports are open. This may take a few minutes.

### Step 4: Research common services running on those ports.

🔹 Host: 192.168.174.1

Open Ports and Services:
Port	State	Service	Description
902	Open	iss-realsecure	VMware ESXi uses this for remote console access (also seen in virtualization).
912	Open	apex-mesh	Part of VMware or similar network service, not very common outside VMs.
3306	Open	MySQL	MySQL database service. Agar yeh port open hai, toh yeh database remotely accessible ho sakta hai.
5357	Open	WSDAPI	Web Services for Devices API – Windows service for device discovery over network.

📌 MAC Address Vendor: VMware – Likely a virtual machine or hypervisor host.
🔹 Host: 192.168.174.2

Open Port:
Port	State	Service
53	Filtered	Domain (DNS)

🔹 Port 53 DNS ke liye hota hai. Iska filtered hone ka matlab hai ki firewall ya IDS is port ko scan ke liye block kar raha hai.
📌 MAC Vendor: VMware again.
🔹 Host: 192.168.174.254

All 1000 ports are filtered.
Yeh machine koi firewall ya security appliance ho sakti hai jo scans ko block kar rahi hai.

📌 MAC Vendor: VMware.
🔹 Host: 192.168.174.131 (Aapka apna device)

    All ports are closed – iska matlab hai ki abhi aapke system me koi listening service nahi chal rahi.

    MAC address match nahi diya, par latency sabse kam hai (aapki machine).

🔍 Summary of Services:
Port	Service	Usage
902	iss-realsecure	VMware remote management / ESXi
912	apex-mesh	VMware related or specialized service
3306	MySQL	Open source relational database, remotely exploitable if misconfigured
5357	WSDAPI	Windows service for device discovery
53	DNS	Domain Name System (possibly protected by firewall)
