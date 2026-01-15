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

<img width="1845" height="860" alt="01-dashboard-overview" src="https://github.com/user-attachments/assets/28d6c181-e7ef-4bcb-8799-0943bbd693a9" />

### Agent active
<img width="929" height="572" alt="02-agents-active" src="https://github.com/user-attachments/assets/01189194-736e-4f4d-a3a0-f7612aa9aaff" />


### Threat Hunting overview
<img width="687" height="293" alt="03-threat-hunting-overview" src="https://github.com/user-attachments/assets/a67d0078-ed38-4d40-a9b8-3dea6b9ed90a" />

<img width="1854" height="970" alt="04-mitre-attack-mapping" src="https://github.com/user-attachments/assets/5784e740-4185-4e20-a424-26f08962a532" />

### MITRE ATT&CK mapping
<img width="1854" height="970" alt="04-mitre-attack-mapping" src="https://github.com/user-attachments/assets/63f70646-4034-4312-a39f-c004ef2845c8" />


### Authentication events (SSH / sudo)

<img width="905" height="461" alt="05-authentication-events" src="https://github.com/user-attachments/assets/497d22d8-0714-45c7-8ad5-03a20cbdc4de" />

### File Integrity Monitoring (FIM) alert (Level 7)
<img width="468" height="924" alt="06-fim-integrity-alert" src="https://github.com/user-attachments/assets/7e85923e-c460-498b-a5a3-0a21f5bfc618" />



## Troubleshooting notes 

- **Self-signed certificate warning** when opening the dashboard: expected in lab environments.
- **Agent/Manager version mismatch:** resolved by aligning versions (agent must be <= manager).
- **`MANAGER_IP` placeholder** in agent config: replaced with the VM IP (`192.168.122.72`).
- **Invalid XML in `ossec.conf`** during changes: fixed by restoring a clean config template and applying minimal changes.
- **Dashboard delay**: agents/events may take some time to appear due to indexing/refresh.

---

## Outcome
This lab demonstrates a working SIEM pipeline with endpoint visibility, authentication monitoring, and high-severity detection via File Integrity Monitoring.

