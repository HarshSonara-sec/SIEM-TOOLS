# Splunk — SOC Essentials: Investigating with Splunk

> **Purpose:** SOC Analyst reference for investigating security events and incidents using Splunk Enterprise Security (ES) and Splunk SOAR.

## 1. Course Overview

**Course:** SOC Essentials: Investigating with Splunk
**Platform:** Splunk Education
**Primary Role:** SOC / Cyber Defense Analyst

The course focuses on conducting security investigations using **Splunk Enterprise Security**, including **Risk-Based Alerting (RBA)**, and introduces common analyst tasks using **Splunk SOAR**.

Splunk describes the course as part of its Defense Analyst learning path. Its investigation workflow covers event triage, investigation, Enterprise Security features, risk-based findings, and SOAR-related response activities.

---

# 2. Splunk Enterprise Security

Splunk Enterprise Security (ES) is the security-focused layer used by SOC teams to investigate events, detect threats, correlate security information, and manage security findings.

Important ES concepts:

* Common Information Model (CIM)
* Data Models
* Data Model Acceleration
* Assets and Identities
* Threat Intelligence
* Detections
* Findings
* Risk-Based Alerting
* Risk findings
* Adaptive Response
* Investigation dashboards

The course specifically emphasizes the relationship between **CIM, Data Models and acceleration**, along with the use of common CIM fields during investigations.

---

# 3. Basic Investigation Workflow

A SOC analyst generally follows a workflow similar to:

```text
Alert / Finding
      ↓
Initial Triage
      ↓
Identify Relevant Events
      ↓
Examine Fields & Context
      ↓
Correlate Related Activity
      ↓
Determine Scope
      ↓
Assess Risk
      ↓
Investigate / Hunt
      ↓
Respond or Escalate
      ↓
Document Findings
```

The exact workflow varies according to the organization's SOC procedures.

---

# 4. Splunk Search & Reporting

The **Search & Reporting** application is the primary interface for searching Splunk data, creating reports, and building dashboards. Splunk uses **Search Processing Language (SPL)** for searches.

Important areas of the Search interface include:

* Search bar
* Time range selector
* Search mode
* Timeline
* Events viewer
* Fields sidebar
* Search results
* Reports / dashboards / alerts

The timeline can help identify spikes or unusual activity, while the fields sidebar provides fields discovered from matching events.

---

# 5. Important Splunk Fields

Fields provide structured information extracted from events.

Common fields include:

| Field            | Purpose                          |
| ---------------- | -------------------------------- |
| `host`           | System that generated the event  |
| `source`         | Source of the event data         |
| `sourcetype`     | Type/classification of the event |
| `src_ip`         | Source IP address                |
| `dest_ip`        | Destination IP address           |
| `src_port`       | Source port                      |
| `dest_port`      | Destination port                 |
| `user`           | Associated username              |
| `action`         | Action performed                 |
| `status`         | Result/status of an operation    |
| `process`        | Associated process               |
| `parent_process` | Parent process                   |
| `file_name`      | Relevant file                    |
| `command`        | Command or command line          |

> **SOC habit:** Do not investigate only the alert description. Extract the important fields and use them to pivot into related events.

---

# 6. SPL Investigation Basics

Splunk searches are commonly built by progressively narrowing and transforming event data.

### Basic search

```spl
index=<index>
```

Search a particular source:

```spl
index=<index> sourcetype=<sourcetype>
```

Search for a specific IP:

```spl
index=<index> src_ip="10.10.10.10"
```

Search for a user:

```spl
index=<index> user="admin"
```

Search for multiple conditions:

```spl
index=<index> src_ip="10.10.10.10" action="failed"
```

Count events:

```spl
index=<index>
| stats count
```

Count events by source IP:

```spl
index=<index>
| stats count by src_ip
```

Sort results:

```spl
index=<index>
| sort - count
```

The Splunk Search Reference provides the complete SPL command and function reference.

---

# 7. Investigation Using Fields

A useful SOC investigation technique is **pivoting**.

Example:

```text
Suspicious IP
     ↓
Search all events containing IP
     ↓
Identify affected hosts
     ↓
Identify usernames
     ↓
Identify processes
     ↓
Identify commands
     ↓
Identify timestamps
     ↓
Search related activity
```

Example:

```spl
index=<index> src_ip="10.10.10.10"
| stats count by user, host, action
```

This can reveal which users and hosts were associated with the activity.

---

# 8. CIM — Common Information Model

The **Common Information Model (CIM)** provides a standardized structure for security-related data.

Why CIM matters:

```text
Different log sources
        ↓
Different formats
        ↓
       CIM
        ↓
Common field structure
        ↓
Consistent searches / detections
```

CIM helps security teams create searches and detections that can work across different data sources.

Important concept:

> **CIM normalization allows different technologies to expose security-relevant information using consistent fields.**

The course specifically identifies CIM and Data Models as important components of Splunk ES investigations.

---

# 9. Data Models

Splunk ES uses Data Models to organize normalized security data.

Examples include data related to:

* Authentication
* Network traffic
* Endpoint activity
* Processes
* Web activity
* Malware
* Vulnerabilities

Data Model Acceleration can improve search performance for supported data models.

---

# 10. Assets & Identities

Splunk ES maintains context about:

### Assets

Examples:

* Servers
* Workstations
* Network devices
* Applications

### Identities

Examples:

* Users
* Employees
* Accounts

This context allows an investigation to consider **who or what** is involved rather than treating every event as an isolated log entry.

Example:

```text
Source IP
   ↓
Hostname
   ↓
Asset information
   ↓
User identity
   ↓
Historical activity
```

The course identifies the Asset & Identity framework as an important ES investigation component.

---

# 11. Threat Intelligence

Threat intelligence can provide additional context about indicators such as:

* IP addresses
* Domains
* URLs
* File hashes
* Other indicators of compromise (IOCs)

Example investigation:

```text
Suspicious IP
      ↓
Threat intelligence lookup
      ↓
Known malicious / suspicious?
      ↓
Search internal activity
      ↓
Determine affected systems
```

> **Important:** An indicator being present in threat intelligence should be treated as investigation context, not automatically as proof that an incident occurred.

---

# 12. Detections

A **detection** identifies activity that meets defined security conditions.

Conceptually:

```text
Event / Data
    ↓
Detection logic
    ↓
Match
    ↓
Finding / Alert
    ↓
Analyst investigation
```

Detection logic can be based on:

* Event fields
* Thresholds
* Correlation
* Known attack patterns
* Behavioral indicators
* Risk signals

---

# 13. Findings

A **Finding** represents security-relevant information generated by Splunk ES.

Important terms from the course include:

* Finding
* Risk-Based Finding
* Intermediate Finding
* Entity
* Adaptive Response Action

These concepts help organize detection and investigation activity inside Enterprise Security.

---

# 14. Risk-Based Alerting (RBA)

Risk-Based Alerting is an important Splunk ES concept.

Traditional model:

```text
Individual Event
      ↓
Detection
      ↓
Alert
```

Risk-based model:

```text
Multiple Risk Signals
        ↓
Entity
        ↓
Risk Accumulation
        ↓
Risk-Based Finding
        ↓
Investigation
```

Instead of treating every suspicious event as an independent high-priority alert, risk-based approaches can accumulate signals associated with an entity.

Possible entities:

* User
* Host
* IP
* Account
* Other security-relevant objects

The course specifically covers the **Risk framework** and **Risk-Based Alerting**.

---

# 15. Adaptive Response

Adaptive Response allows security actions to be associated with detections.

Conceptually:

```text
Detection
   ↓
Adaptive Response
   ↓
Action
```

Possible actions can involve:

* Gathering additional information
* Enriching an event
* Sending information to another security system
* Initiating an automated workflow

Adaptive Response is particularly relevant when Splunk ES works together with SOAR.

---

# 16. Splunk SOAR

**SOAR = Security Orchestration, Automation and Response**

Splunk SOAR is designed to help security teams automate and orchestrate response workflows.

Basic model:

```text
Alert
  ↓
SOAR
  ↓
Playbook
  ↓
Automated actions
  ↓
Investigation / Response
```

A **playbook** defines the workflow used to perform automated actions.

Examples of automation concepts:

```text
Receive IOC
    ↓
Enrich IOC
    ↓
Check reputation
    ↓
Collect additional information
    ↓
Take response action
    ↓
Update incident
```

The Splunk course introduces SOAR playbooks and ways they can be triggered from Enterprise Security.

---

# 17. SOC Investigation Checklist

When investigating an alert:

### 1. Identify the alert

* What triggered it?
* Which detection generated it?
* When did it occur?
* What is the severity?

### 2. Identify the entity

* Which host?
* Which user?
* Which IP?
* Which account?
* Which application?

### 3. Examine the event

* Source
* Destination
* Timestamp
* Process
* Command
* Action
* Status
* Authentication details

### 4. Pivot

Search for:

* Same IP
* Same user
* Same hostname
* Same process
* Same hash
* Same domain
* Related events before/after the alert

### 5. Establish scope

Determine:

* Number of affected hosts
* Number of affected accounts
* Duration of activity
* Related indicators
* Whether other systems show similar activity

### 6. Determine next action

Possible outcomes:

* Benign / expected activity
* False positive
* Suspicious activity requiring further investigation
* Confirmed security incident
* Escalation to L2 / incident response

### 7. Document

Record:

* Alert
* Evidence
* Timeline
* Indicators
* Affected assets
* Investigation performed
* Conclusion
* Response/escalation

---

# 18. Key Takeaways

For SOC L1 work, remember:

```text
Alert ≠ Incident
```

An alert is a signal that requires investigation.

A good analyst:

1. Understands the alert.
2. Extracts important fields.
3. Searches related events.
4. Pivots using indicators.
5. Correlates activity.
6. Determines scope.
7. Evaluates risk.
8. Documents evidence.
9. Escalates or responds according to the SOC procedure.

### Official References

* Splunk SOC Essentials: Investigating with Splunk — official course description.
* Splunk Search Tutorial — official documentation.
* Splunk Search Reference — official SPL reference.
