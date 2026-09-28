# MITRE ATT&CK

## What is MITRE ATT&CK?

MITRE ATT&CK is a globally used knowledge base that documents real\-world adversary behavior\. It organizes attacker activity into **tactics, techniques, and sub\-techniques** so defenders can understand how attacks are carried out and map security detections and investigations to known behaviors\.

The framework is useful for SOC analysts, threat hunters, detection engineers, incident responders, and security engineers\.

## Core Concepts

### Tactics

Tactics describe the **attacker’s objective** — the reason an adversary performs an action\.

Examples include:

- Reconnaissance
- Resource Development
- Initial Access
- Execution
- Persistence
- Privilege Escalation
- Defense Evasion
- Credential Access
- Discovery
- Lateral Movement
- Collection
- Command and Control
- Exfiltration
- Impact

### Techniques

Techniques describe **how an attacker achieves a tactical objective**\.

Examples:

- Phishing
- Valid Accounts
- Command and Scripting Interpreter
- PowerShell
- Remote Services
- OS Credential Dumping
- Data from Local System

### Sub\-techniques

Sub\-techniques provide more specific descriptions of techniques\.

For example, PowerShell is a sub\-technique under **Command and Scripting Interpreter**\.

## How SOC Teams Use MITRE ATT&CK

A SOC can use ATT&CK to:

1. Map security alerts to known attacker behaviors\.
2. Build and improve detection rules\.
3. Identify gaps in monitoring coverage\.
4. Support threat hunting\.
5. Structure incident investigations\.
6. Understand an adversary’s likely attack path\.
7. Communicate incidents using a common security vocabulary\.
8. Measure detection coverage across tactics and techniques\.

## Example

Suppose a SIEM generates an alert showing suspicious PowerShell execution\.

A SOC analyst can:

1. Investigate the PowerShell command\.
2. Determine whether the activity is legitimate\.
3. Map the behavior to the relevant ATT&CK technique/sub\-technique\.
4. Search for related activity such as persistence, credential access, or lateral movement\.
5. Escalate or contain the incident if malicious behavior is confirmed\.

## ATT&CK and Detection Engineering

MITRE ATT&CK should not be treated as a simple checklist\. A mature SOC uses it to connect:

**Threat behavior → Telemetry → Detection → Investigation → Response**

For example:

- **Behavior:** suspicious PowerShell execution
- **Telemetry:** Windows process and PowerShell logs
- **Detection:** SIEM correlation/search rule
- **Investigation:** parent process, user, host, command line, network activity
- **Response:** contain host, investigate scope, remove persistence, document findings

## Important Takeaway

MITRE ATT&CK describes **what adversaries do**, while a SOC uses that knowledge to determine **what telemetry to collect, what to detect, and how to investigate suspicious activity**\.

## Useful Resources

- MITRE ATT&CK: https://attack\.mitre\.org/
- MITRE ATT&CK Navigator: https://mitre\-attack\.github\.io/attack\-navigator/
