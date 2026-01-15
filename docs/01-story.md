docs/01-story.md# Wazuh SIEM Lab – Project Story

## Context

As part of my cybersecurity learning path, I wanted to move beyond theory and build a *docs/01-story.md*real SIEM environment** capable of collecting, correlating, and visualizing security events from an endpoint.

The objective was not only to make Wazuh work, but to **operate it like a blue team analyst**: deploy it, break it, fix it, and validate detections with real system activity.

---

## Problem

Most beginner labs stop at "installation completed".

In real environments, security engineers face issues such as:

* Agent–manager incompatibilities
* Broken configurations
* Certificate warnings
* Delayed or missing alerts
* Confusing dashboards and timestamps

I wanted to replicate that reality and document the full process.

---

## Solution

I designed and deployed a Wazuh SIEM lab with:

* A **dedicated Wazuh server** running in a virtual machine
* A **Linux endpoint** monitored with a Wazuh agent
* Secure communication and key-based agent enrollment
* Active monitoring of authentication, processes, network ports, and files

The lab was intentionally stressed by:

* Restarting services
* Modifying monitored files
* Generating authentication and sudo activity

---

## Implementation Summary

* Installed and validated Wazuh Manager, Indexer, and Dashboard
* Enrolled endpoint agent using `manage_agents`
* Configured File Integrity Monitoring (FIM)
* Verified alert ingestion and indexing
* Analyzed alerts in Threat Hunting view
* Reviewed MITRE ATT&CK mappings

---

## Evidence-Based Validation

The following detections were successfully generated and analyzed:

* SSH authentication events (success and session closure)
* Privilege escalation via sudo
* File integrity changes (checksum modification)
* Network port state changes

Each detection is supported by screenshots and exported data stored in the `evidence/` directory.

---

## Challenges & Lessons Learned

* **Version mismatches** between agent and manager prevent enrollment
* A single malformed XML tag in `ossec.conf` can break the agent entirely
* Placeholder values (e.g., `MANAGER_IP`) must be explicitly replaced
* Alert visualization may lag behind event generation
* Understanding rule IDs and severity is critical for proper triage

These issues were resolved through log analysis, service inspection, and iterative testing.

---

## Outcome

This project resulted in a **fully operational SIEM lab** capable of detecting and correlating endpoint activity.

More importantly, it strengthened my ability to:

* Troubleshoot security tooling under pressure
* Validate detections using evidence
* Communicate technical work clearly

---

## Why This Matters

This lab reflects real blue team work:

* Tools do not work perfectly
* Logs must be interpreted, not just collected
* Evidence and documentation are as important as alerts

This repository demonstrates my readiness to contribute to a security operations environment as a junior analyst.
