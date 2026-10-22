# Local SOC & SIEM Home Lab

## Project Overview
This repository documents the design and deployment of a localized, enterprise-style Security Operations Center (SOC) laboratory. The lab is built to simulate real-world threat activity, generate high-fidelity endpoint telemetry, and analyze security events using a local SIEM pipeline.

The environment utilizes an attacker node (Kali Linux) and a target endpoint (Windows 11 Pro) hosted on an Arch-based CachyOS system, streaming security logs directly into a local Elasticsearch & Kibana SIEM stack running via Docker.

---

## Architecture & Core Components

* **Host Infrastructure:** CachyOS (Arch Linux-based, x86_64)
* **Hypervisor Platform:** Oracle VM VirtualBox
* **Attacker Node (Red Team):** Kali Linux
* **Target Endpoint (Blue Team):** Windows 11 Pro
* **SIEM & Analytics Engine:** Elasticsearch & Kibana 8.12.0 (Dockerized)

* **Screenshot Placeholder:**  
  `![Virtualization Environment Topology](./images/phase1-vbox-topology.png)
  

---

## Host Firewall & Network Isolation
To secure the SOC management layer and protect internal SIEM endpoints (such as the Elasticsearch ingestion port `9200`) from unauthorized external network access, a strict host-based firewall policy was enforced on CachyOS:

1. **Default Ingress Boundary:** Configured the host firewall (`ufw`) default inbound policy to **Ignore** (silent packet drop) to conceal management ports from network discovery scans.
2. **Access Control Management:** Restricted external network access while maintaining internal communication between the host SIEM stack and virtual machine interfaces.

```bash
# Remove temporary exposure rules
sudo ufw delete allow 9200/tcp

# Verify active firewall status
sudo ufw status verbose
