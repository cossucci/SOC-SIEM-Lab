# Local SOC & SIEM Home Lab

## Phase 1: Lab Architecture & Endpoint Instrumentation

### Overview
Phase 1 establishes the virtual networking infrastructure, target Windows 11 endpoint, and baseline endpoint telemetry generation required for host-level visibility in the SOC environment.

---

### Hypervisor Topology & Virtual Networking
1. Launch **Oracle VM VirtualBox**.
2. Configure virtual network adapters ensuring the target VM and analyst/attacker VM operate in a controlled lab environment.
3. Verify VM operational state:
   * **Target VM:** Windows 11 Pro (Blue Team / Log Source)
   * **Attacker VM:** Kali Linux (Red Team)

![VirtualBox Topology](./images/phase1-vbox-topology.png)

---

### Target Endpoint Provisioning & Telemetry Setup
System Monitor (Sysmon) provides high-fidelity host logging (process creation, network connections, file modifications) essential for SIEM detection and threat hunting.

1. Open **PowerShell as Administrator** inside the Windows 11 VM.
2. Create the working directory, fetch the Sysmon binary along with the SwiftOnSecurity baseline configuration, and execute installation:

```powershell
# Create dedicated working directory
mkdir C:\Sysmon
cd C:\Sysmon

# Fetch Sysmon64 executable
Invoke-WebRequest -Uri "[https://live.sysinternals.com/Sysmon64.exe](https://live.sysinternals.com/Sysmon64.exe)" -OutFile "Sysmon64.exe"

# Fetch SwiftOnSecurity baseline configuration
Invoke-WebRequest -Uri "[https://raw.githubusercontent.com/SwiftOnSecurity/sysmon-config/master/sysmonconfig-export.xml](https://raw.githubusercontent.com/SwiftOnSecurity/sysmon-config/master/sysmonconfig-export.xml)" -OutFile "sysmonconfig-export.xml"

# Execute service installation with configuration
.\Sysmon64.exe -i sysmonconfig-export.xml -accepteula

