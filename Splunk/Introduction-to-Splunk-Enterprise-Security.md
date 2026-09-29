# Splunk — Introduction to Enterprise Security

> **Course:** Introduction to Enterprise Security
> **Platform:** Splunk Education
> **Completion:** 29 September 2026
> **Credit Hours:** 1
> **Status:** Completed / Certificate Earned

---

## 1. What is Splunk Enterprise Security?

**Splunk Enterprise Security (ES)** is a security-focused solution built on the Splunk platform.

It is designed to help security teams monitor, investigate, detect, and respond to security threats across enterprise environments.

For a SOC analyst, Enterprise Security brings security information together so analysts can investigate suspicious activity from a centralized security platform.

---

# 2. Splunk Enterprise Security in a SOC

A typical SOC workflow can be represented as:

```text
Security Data
      ↓
Splunk Platform
      ↓
Enterprise Security
      ↓
Detection / Alert
      ↓
Investigation
      ↓
Incident Analysis
      ↓
Response
```

The purpose is to transform large amounts of security telemetry into information that analysts can use.

---

# 3. Security Data

Enterprise security monitoring can involve data from many systems.

Examples include:

* Endpoint systems
* Servers
* Firewalls
* Network devices
* Authentication systems
* Applications
* Cloud platforms
* DNS infrastructure
* Intrusion Detection Systems
* Intrusion Prevention Systems
* Endpoint Detection and Response platforms

The more useful and correctly normalized the data is, the more effectively security content can operate.

---

# 4. Security Operations

Security operations involve continuously monitoring an environment for suspicious or malicious activity.

A simplified SOC process is:

```text
Monitor
   ↓
Detect
   ↓
Investigate
   ↓
Analyze
   ↓
Respond
   ↓
Document
```

Splunk Enterprise Security supports this workflow by providing security-focused views and investigation capabilities.

---

# 5. Notable Events

A security monitoring system can generate many events.

A **notable event** represents an event or detection that deserves investigation.

Examples:

```text
Repeated failed authentication
Suspicious network connection
Malware detection
Unusual account activity
Suspicious process execution
Potential data exfiltration
```

A SOC analyst evaluates the context around the event rather than immediately assuming that every alert represents a confirmed attack.

---

# 6. Alerts vs. Events

### Event

An event is an individual piece of recorded activity.

Example:

```text
user=admin
action=login_failed
src_ip=192.168.1.50
```

### Alert

An alert is generated when defined detection logic identifies activity that requires attention.

Conceptually:

```text
Many Events
     ↓
Detection Logic
     ↓
Alert
     ↓
Analyst Investigation
```

---

# 7. Incident Investigation

When an alert is generated, an analyst needs to investigate the surrounding activity.

Important questions include:

```text
Who?
What?
When?
Where?
How?
What happened before?
What happened after?
Is this legitimate?
```

For example:

```text
Alert
 ↓
Source IP
 ↓
User
 ↓
Host
 ↓
Previous activity
 ↓
Related activity
 ↓
Timeline
```

---

# 8. Correlation

**Correlation** means connecting related security events to identify a larger activity pattern.

Example:

```text
Failed Login
      +
Successful Login
      +
New Host Access
      +
Privilege Change
      ↓
Potential Account Compromise
```

Each individual event may not be enough to determine what happened.

Correlation provides additional context.

---

# 9. Risk-Based Thinking

Security operations should consider the total context of activity.

A single low-level event may not require the same attention as multiple related suspicious events.

Conceptually:

```text
Event A → Low Risk
Event B → Low Risk
Event C → Suspicious
Event D → Suspicious
       ↓
Combined Context
       ↓
Higher Investigation Priority
```

This approach helps analysts focus on meaningful security activity.

---

# 10. Entities

An **entity** is an object involved in security activity.

Examples:

```text
User
Host
IP Address
Device
Account
```

An investigation can focus on an entity and examine its associated activity.

Example:

```text
User: admin
    ↓
Login attempts
    ↓
Source IPs
    ↓
Hosts accessed
    ↓
Commands / activity
```

---

# 11. Source and Destination

Network security investigations frequently involve:

```text
Source → Destination
```

Example:

```text
192.168.1.20 → 10.0.0.50
```

Where:

```text
Source = system initiating communication
Destination = system receiving communication
```

Important fields may include:

```text
src_ip
dest_ip
src_port
dest_port
```

These fields are frequently used when investigating network activity.

---

# 12. Security Investigation with SPL

Splunk Search Processing Language (**SPL**) can be used to investigate events.

Example:

```spl
index=security
| table _time user src_ip dest_ip action
```

Group events:

```spl
index=security
| stats count by src_ip
| sort - count
```

Search for a particular source:

```spl
index=security src_ip=192.168.1.100
```

The analyst can then investigate the activity associated with that source.

---

# 13. Time-Based Investigation

Time is extremely important during incident investigation.

An analyst may need to establish:

```text
First suspicious activity
        ↓
Initial access
        ↓
Additional activity
        ↓
Detection
        ↓
Response
```

This creates an incident timeline.

Useful SPL field:

```text
_time
```

Example:

```spl
index=security
| sort _time
| table _time user src_ip host action
```

---

# 14. Security Frameworks

Security detections can be mapped to frameworks that help analysts understand attacker behavior.

Important framework:

**MITRE ATT&CK**

MITRE ATT&CK describes attacker tactics and techniques.

Examples:

```text
Initial Access
Execution
Persistence
Privilege Escalation
Defense Evasion
Credential Access
Discovery
Lateral Movement
Collection
Command and Control
Exfiltration
Impact
```

Security Essentials content can associate detections with MITRE ATT&CK techniques, providing useful context for security monitoring.

---

# 15. Common Information Model

**CIM = Common Information Model**

CIM provides standardized field names and data structures in Splunk.

This is important because security data can come from many different products.

For example:

```text
Firewall
Endpoint
Cloud
DNS
Authentication
     ↓
Different data formats
     ↓
CIM normalization
     ↓
Consistent security searches
```

Standardized data makes security content easier to reuse across environments.

---

# 16. Security Dashboards

Dashboards allow analysts to visualize security information.

Examples of useful information include:

* Security events
* Alerts
* Authentication activity
* Network activity
* Endpoint activity
* Threat activity
* Detection coverage
* Risk information

Dashboards are particularly useful for quickly understanding the state of an environment.

---

# 17. Detection and Investigation

A detection is only the beginning of an investigation.

Example:

```text
Detection:
Suspicious Login
      ↓
Who?
      ↓
Source IP?
      ↓
Destination Host?
      ↓
Previous Login Activity?
      ↓
Other Activity?
      ↓
Legitimate or Suspicious?
      ↓
Document Findings
```

A SOC analyst should avoid investigating an alert in isolation when additional context is available.

---

# 18. False Positives

A **false positive** occurs when legitimate activity triggers a security detection.

Examples:

```text
Administrator performing maintenance
Automated service account activity
Scheduled security scanner
User entering an incorrect password
Authorized vulnerability scanning
```

The analyst needs to determine whether the activity is expected or suspicious.

---

# 19. Security Analyst Mindset

A SOC analyst should approach an alert using evidence.

Instead of asking only:

```text
"Is this an attack?"
```

Ask:

```text
What happened?
Who was involved?
When did it happen?
Where did it happen?
What evidence supports the detection?
What happened before it?
What happened after it?
Is there a legitimate explanation?
What additional data should be checked?
```

This creates a structured investigation process.

---

# 20. Example SOC Investigation

Suppose Enterprise Security identifies suspicious authentication activity.

### Step 1 — Identify the user

```spl
index=security action=failure
| stats count by user
| sort - count
```

### Step 2 — Identify source IPs

```spl
index=security action=failure
| stats count by src_ip
| sort - count
```

### Step 3 — Investigate a source

```spl
index=security src_ip=192.168.1.100
| table _time user host action
```

### Step 4 — Build a timeline

```spl
index=security src_ip=192.168.1.100
| sort _time
| table _time user host action
```

### Step 5 — Correlate

Look for:

```text
Authentication
+
Endpoint activity
+
Network activity
+
Account changes
+
Other alerts
```

The goal is to determine what happened using evidence from multiple events.

---

# 21. Key Enterprise Security Concepts

```text
Security Data
      ↓
Normalization
      ↓
Detection
      ↓
Notable Event / Alert
      ↓
Entity Investigation
      ↓
Correlation
      ↓
Timeline
      ↓
Incident Analysis
      ↓
Response
```

Important concepts to remember:

* Security monitoring
* Security events
* Alerts
* Notable events
* Entities
* Correlation
* Risk-based investigation
* Incident investigation
* Security dashboards
* SPL
* CIM
* MITRE ATT&CK
* False positives
* Security data sources

---

# 22. SOC Analyst Takeaways

This introductory Enterprise Security course establishes the relationship between Splunk and enterprise security operations.

The important skills to carry forward are:

1. Understand security events.
2. Understand how detections generate alerts.
3. Investigate alerts using SPL.
4. Identify users, hosts, and IP addresses.
5. Correlate related events.
6. Build timelines.
7. Consider the context around an alert.
8. Identify potential false positives.
9. Understand normalized security data.
10. Use security dashboards to obtain an overview.
11. Understand how MITRE ATT&CK can provide attacker-behavior context.
12. Think like an analyst rather than simply searching for individual logs.

---

## Certificate

**Introduction to Enterprise Security — Certificate of Completion**

Completed:

**29 September 2026**

**1 Credit Hour**
