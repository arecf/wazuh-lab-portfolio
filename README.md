# Wazuh SIEM Home Lab (Ubuntu + VM)

This repository documents a Wazuh SIEM lab deployed with:
- **Wazuh Server** on an Ubuntu VM (Manager + Indexer + Dashboard)
- **Wazuh Agent** on the Ubuntu host machine (endpoint)
- Evidence of **authentication events** and **high-severity alerts** such as **File Integrity Monitoring (FIM)**.

---

## Architecture

**Network:** libvirt NAT `192.168.122.0/24`  
**Server (VM):** `192.168.122.72`  
**Endpoint (Host):** Ubuntu host running the agent

High-level flow:

`Endpoint (Agent) → Manager (rules) → Indexer (storage) → Dashboard (visualization)`

---

## Components
- **wazuh-manager**: analyzes logs, applies rules, generates alerts
- **wazuh-indexer (OpenSearch)**: stores alerts/events
- **wazuh-dashboard**: web UI for investigation and reporting
- **wazuh-agent**: collects endpoint telemetry (auth logs, integrity events, etc.)

---

## What was validated

### 1) Agent connectivity
- Agent successfully enrolled using **agent keys**
- Agent appears as **Active** in the dashboard

### 2) Authentication monitoring
- SSH / PAM activity and sudo usage were collected and displayed as events

### 3) High-severity detection (FIM)
- File Integrity Monitoring was configured to watch a test directory
- A file change triggered an alert:
  - **Rule:** `Integrity checksum changed`
  - **Severity:** **Level 7**
  - Demonstrates detection of unauthorized modification/tampering

---

## Evidence (Screenshots)

### Dashboard overview
file:///home/arecf/evidence/screenshots/01-dashboard-overview.png

### Agent active
![Agent active](evidence/screenshots/02-agents-active.png)

### Threat Hunting overview
![Threat Hunting overview](evidence/screenshots/03-threat-hunting-overview.png)

### MITRE ATT&CK mapping
![MITRE mapping](evidence/screenshots/04-mitre-attack-mapping.png)

### Authentication events (SSH / sudo)
![Authentication events](evidence/screenshots/05-authentication-events.png)

### File Integrity Monitoring (FIM) alert (Level 7)
![FIM alert](evidence/screenshots/06-fim-integrity-alert.png)

### FIM alert details
![FIM details](evidence/screenshots/07-fim-alert-details.png)

### Network monitoring / ports alert
![Network alert](evidence/screenshots/08-network-monitoring-alert.png)

---

## Troubleshooting notes 

- **Self-signed certificate warning** when opening the dashboard: expected in lab environments.
- **Agent/Manager version mismatch:** resolved by aligning versions (agent must be <= manager).
- **`MANAGER_IP` placeholder** in agent config: replaced with the VM IP (`192.168.122.72`).
- **Invalid XML in `ossec.conf`** during changes: fixed by restoring a clean config template and applying minimal changes.
- **Dashboard delay**: agents/events may take some time to appear due to indexing/refresh.

---

## Outcome
This lab demonstrates a working SIEM pipeline with endpoint visibility, authentication monitoring, and high-severity detection via File Integrity Monitoring.

