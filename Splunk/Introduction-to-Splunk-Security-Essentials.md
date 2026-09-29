# Splunk — Introduction to Splunk Security Essentials

> **Course:** Introduction to Splunk Security Essentials
> **Platform:** Splunk Education
> **Completion:** 27 September 2026
> **Credit Hours:** 1
> **Status:** Completed / Certificate Earned

---

## 1. What is Splunk Security Essentials?

**Splunk Security Essentials (SSE)** is a free Splunk application designed to help security teams discover, understand, implement, and evaluate security monitoring and detection use cases.

It provides security-focused content, detection searches, implementation guidance, response information, and mappings to security frameworks such as **MITRE ATT&CK** and the **Cyber Kill Chain**.

Splunk documents SSE as containing more than 120 detection searches, with line-by-line explanations of the SPL used in detections.

---

# 2. Why Security Essentials is Important for a SOC Analyst

A **Security Operations Center (SOC)** monitors systems and investigates suspicious activity.

Splunk Security Essentials helps connect Splunk searches with real-world security operations.

A SOC analyst can use it to understand:

* What security data is available
* What security detections can be created
* Which SPL searches can detect suspicious behavior
* What data sources are required
* What a detection means
* How an analyst could respond
* Which MITRE ATT&CK techniques are associated with a detection
* Which security use cases are covered by the organization's monitoring

### Basic workflow

```text
Security Data
      ↓
Splunk
      ↓
SPL Search
      ↓
Detection
      ↓
Investigation
      ↓
Response
```

---

# 3. Security Use Cases

A **security use case** describes a security monitoring or detection requirement.

Examples include:

* Brute-force attacks
* Account compromise
* Malware activity
* Suspicious authentication
* Network reconnaissance
* Data exfiltration
* Endpoint activity
* DNS activity
* Web attacks
* Insider threats

The purpose is to translate a security concern into something that can be monitored using available data.

---

# 4. Security Content

The **Security Content** area provides security detection content that can be filtered and explored.

Security content can contain information such as:

* Detection name
* Security use case
* Description
* Search
* Data sources
* Severity
* Implementation guidance
* Response guidance
* Known false positives
* MITRE ATT&CK mapping

This makes the detection useful not only as an SPL query but also as documented security logic.

---

# 5. Detection Searches

A **detection search** is an SPL search designed to identify potentially suspicious behavior.

For example:

```spl
index=security action=failure
| stats count by user src_ip
| sort - count
```

This can help identify users and source IP addresses associated with repeated failures.

The exact detection depends on:

* Available data
* Fields
* Data model
* Detection logic
* Thresholds
* Environment

SSE provides documented detection searches that can be studied and adapted.

---

# 6. SPL — Splunk Search Processing Language

**SPL = Splunk Search Processing Language**

SPL is the language used to search, manipulate, analyze, and visualize data in Splunk.

Basic search:

```spl
index=security
```

Search using fields:

```spl
index=security src_ip=192.168.1.10
```

Filter events:

```spl
index=security action=blocked
```

Aggregate events:

```spl
index=security
| stats count by src_ip
```

Sort results:

```spl
| sort - count
```

---

# 7. Important SPL Commands

### `search`

Filters events based on search conditions.

```spl
index=security action=blocked
```

### `stats`

Performs statistical calculations.

```spl
| stats count by src_ip
```

### `table`

Displays selected fields.

```spl
| table _time src_ip dest_ip user action
```

### `sort`

Sorts results.

```spl
| sort - count
```

### `dedup`

Removes duplicate results based on a field.

```spl
| dedup src_ip
```

### `rename`

Renames a field.

```spl
| rename src_ip AS "Source IP"
```

---

# 8. Fields in Security Investigations

Important fields frequently used by SOC analysts include:

```text
_time
host
source
sourcetype
index
src_ip
dest_ip
src_port
dest_port
user
action
status
```

Fields allow an analyst to move from raw logs to meaningful security information.

Example:

```spl
index=security
| table _time user src_ip dest_ip action
```

---

# 9. Time-Series Searches

SSE can use **time-series searches** to identify unusual changes or spikes in activity.

A time-series analysis can compare activity over time and identify values that significantly differ from normal behavior.

For example:

```text
Normal login activity
        ↓
Sudden increase
        ↓
Potential anomaly
        ↓
Investigate
```

SSE documentation describes time-series searches using statistical analysis such as standard deviation to identify outliers.

---

# 10. First-Time-Seen Detection

Another useful detection concept is identifying something that has **never been observed before**.

Examples:

* A user logging into a new system
* A new source IP
* A new process
* A new destination
* A new domain
* A new file hash

Conceptually:

```text
Previously observed values
          ↓
New value appears
          ↓
First-time activity
          ↓
Investigate
```

SSE supports first-time-seen searches using SPL functionality such as `stats`, `first()`, `last()`, and `eventstats`.

---

# 11. MITRE ATT&CK

**MITRE ATT&CK = MITRE Adversarial Tactics, Techniques, and Common Knowledge**

MITRE ATT&CK provides a structured knowledge base describing attacker tactics and techniques.

Security detections can be mapped to ATT&CK.

Examples of tactics include:

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

SSE provides MITRE ATT&CK mappings for its security content.

---

# 12. Cyber Kill Chain

The **Cyber Kill Chain** is a model used to describe stages of a cyber attack.

A traditional sequence includes:

```text
Reconnaissance
        ↓
Weaponization
        ↓
Delivery
        ↓
Exploitation
        ↓
Installation
        ↓
Command and Control
        ↓
Actions on Objectives
```

SSE provides security content mapped to the Cyber Kill Chain as well as MITRE ATT&CK.

---

# 13. Data Sources

Security detections depend on having the correct data.

Common security data sources include:

* Windows logs
* Linux logs
* Firewall logs
* DNS logs
* Proxy logs
* Endpoint Detection and Response (EDR) data
* Intrusion Detection System (IDS) logs
* Intrusion Prevention System (IPS) logs
* Authentication logs
* Cloud logs
* Application logs
* Network traffic

If the required data is not available, a detection may not work correctly.

SSE can help identify data sources and check whether required security data exists in the environment.

---

# 14. Dashboards

Dashboards provide a visual representation of security information.

Examples:

* Security events
* Network activity
* Authentication failures
* Detection results
* MITRE ATT&CK coverage
* Cyber Kill Chain coverage
* Security posture

SSE can create security posture dashboards based on available data sources.

---

# 15. Security Posture

**Security posture** represents the organization's overall security condition and monitoring coverage.

A security posture dashboard can help answer questions such as:

```text
What security data do we have?
What detections are available?
What security areas are covered?
What areas have limited coverage?
```

This gives security teams a broader view instead of investigating individual events only.

---

# 16. Common Information Model

**CIM = Common Information Model**

Splunk's Common Information Model provides standardized field names and structures that allow data from different sources to be used consistently.

Example concept:

```text
Firewall A
      ↓
Firewall B
      ↓
Cloud Firewall
      ↓
Standardized Security Data
```

Standardization makes searches and security content easier to reuse across environments.

SSE includes functionality for checking whether data is **CIM compliant**.

---

# 17. Risk-Based Alerting

**RBA = Risk-Based Alerting**

Risk-based alerting focuses on accumulating evidence and risk rather than treating every individual event as an independent high-priority alert.

Conceptually:

```text
Event 1 → Low risk
Event 2 → Low risk
Event 3 → Suspicious
        ↓
Combined evidence
        ↓
Higher investigation priority
```

This approach can help reduce alert overload and provide more context around suspicious entities.

---

# 18. False Positives

A **false positive** occurs when a detection identifies activity as suspicious even though the activity is legitimate.

Example:

```text
Detection:
Multiple failed logins

Possible explanation:
User forgot password
```

Therefore, detection content should consider:

* Known legitimate activity
* Expected administrative behavior
* Service accounts
* Automated systems
* Scheduled tasks
* Business processes

SSE detection content includes information about known false positives and response considerations.

---

# 19. Investigation Questions

When investigating an alert, ask:

### Who?

```text
Which user or account?
```

### What?

```text
What activity occurred?
```

### When?

```text
When did it occur?
```

### Where?

```text
Which host or system?
```

### Source?

```text
What was the source IP?
```

### Destination?

```text
What system or service was targeted?
```

### Why?

```text
Is there a legitimate explanation?
```

---

# 20. SOC Investigation Example

Suppose a detection reports repeated failed logins.

Start with:

```spl
index=security action=failure
| table _time user src_ip host action
```

Then identify the most active source IPs:

```spl
index=security action=failure
| stats count by src_ip
| sort - count
```

Investigate one source:

```spl
index=security src_ip=192.168.1.100
| table _time user host action
```

Then correlate the activity with other events.

---

# 21. Important SSE Concepts Learned

```text
Splunk Security Essentials
        ↓
Security Content
        ↓
Detection Searches
        ↓
SPL
        ↓
Data Sources
        ↓
Security Use Cases
        ↓
MITRE ATT&CK / Cyber Kill Chain
        ↓
Investigation & Response
```

---

# 22. SOC Analyst Takeaways

After completing this course, the important skills/concepts are:

* Understanding what Splunk Security Essentials provides
* Understanding security detection content
* Reading SPL detection searches
* Understanding security fields
* Searching security events
* Aggregating events with `stats`
* Understanding first-time-seen detection
* Understanding time-series anomaly detection
* Understanding security data sources
* Understanding false positives
* Understanding MITRE ATT&CK mappings
* Understanding Cyber Kill Chain mappings
* Understanding security dashboards
* Understanding CIM
* Understanding risk-based alerting
* Thinking about detections from an investigation perspective

---

## Certificate

**Introduction to Splunk Security Essentials — Certificate of Completion**

Completed:

**27 September 2026**

**1 Credit Hour**
# Splunk — Introduction to Splunk Security Essentials

## Overview

Splunk Security Essentials is designed to help security teams understand and use Splunk for security monitoring, investigation, and detection.

It provides security-focused searches, dashboards, use cases, and guidance that can help analysts work with security data.

---

## 1. Security Operations with Splunk

Splunk can be used throughout the security operations process:

```text
Data Collection
      ↓
Monitoring
      ↓
Detection
      ↓
Investigation
      ↓
Response
      ↓
Continuous Improvement
```

For a SOC analyst, Splunk can serve as a central platform for searching and analyzing security-related events.

---

## 2. Security Use Cases

Security monitoring can cover areas such as:

* Authentication
* Network activity
* Endpoint activity
* Malware
* Vulnerability information
* Firewall events
* Intrusion detection
* Web activity
* Cloud activity
* Data access
* Threat detection

Different security data sources can be searched and correlated to investigate suspicious activity.

---

## 3. Security Data

Useful security data can originate from many sources.

Examples:

```text
Firewalls
IDS/IPS
Endpoints
Servers
Authentication systems
Applications
Network devices
Cloud services
Security tools
```

Splunk can bring this information together so analysts can search across different sources.

---

## 4. Security Use Cases and Detection

A security use case describes a security problem that needs to be monitored or detected.

Example:

### Brute-Force Login Detection

Relevant information may include:

```text
user
src_ip
action
status
_time
```

An analyst can search for repeated authentication failures and investigate the source.

Example SPL:

```spl
index=security action=failure
| stats count by user src_ip
| sort - count
```

---

## 5. Dashboards

Security dashboards can provide a visual overview of important security activity.

Examples of information that can be displayed:

* Number of security events
* Authentication failures
* Top source IPs
* Security alerts
* Network activity
* Malware activity
* Event trends

Dashboards can help SOC analysts quickly identify unusual activity.

---

## 6. Security Investigation

Splunk searches can be used to investigate an alert.

A basic investigation workflow:

```text
Alert
 ↓
Identify relevant events
 ↓
Examine fields
 ↓
Identify source/user/host
 ↓
Search related activity
 ↓
Build timeline
 ↓
Determine whether activity is suspicious
```

Useful investigation fields may include:

```text
_time
src_ip
dest_ip
user
host
action
status
```

---

## 7. Security Essentials and MITRE ATT&CK

Security monitoring can be mapped to adversary behavior and techniques.

The MITRE ATT&CK framework provides a structured way of describing attacker tactics and techniques.

Examples of areas monitored by a SOC include:

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

Mapping detections to attacker techniques helps security teams understand what behavior their monitoring can detect.

---

## 8. Common SOC Questions

When investigating security data in Splunk, an analyst may ask:

### Who?

```text
Which user account was involved?
```

### What?

```text
What action occurred?
```

### Where?

```text
Which host or system was involved?
```

### When?

```text
When did the activity occur?
```

### Where did it originate?

```text
What was the source IP?
```

### What was targeted?

```text
What destination host or service was involved?
```

These questions help structure an investigation.

---

## 9. Example Investigation

Suppose a SOC analyst receives multiple failed-login alerts.

Start by examining the relevant fields:

```spl
index=security action=failure
| table _time user src_ip host action
```

Then identify the most active sources:

```spl
index=security action=failure
| stats count by src_ip
| sort - count
```

Investigate a suspicious source:

```spl
index=security src_ip=192.168.1.100
| table _time user host action
```

The analyst can then correlate the activity with other events.

---

## 10. Why Security Essentials Matters for a SOC Analyst

Splunk is more than a log-search platform in a SOC environment.

A SOC analyst needs to understand how to:

* Search security data
* Identify important fields
* Filter events
* Detect suspicious behavior
* Investigate alerts
* Build timelines
* Correlate related events
* Understand security use cases
* Communicate investigation findings

---

## SOC Analyst Takeaway

The important concept from Splunk Security Essentials is the connection between **Splunk capabilities and real security operations**.

```text
Security Data
     ↓
Splunk
     ↓
Search & Detection
     ↓
Investigation
     ↓
Security Decision
     ↓
Response
```

For an entry-level SOC analyst, focus on becoming comfortable with:

1. Searching events
2. Understanding fields
3. Filtering data
4. Using `stats`
5. Investigating IP addresses
6. Investigating users and hosts
7. Understanding security use cases
8. Reading security dashboards
9. Thinking in terms of attacker behavior
10. Building an investigation timeline

**Certificate:** Completed
**Course:** Introduction to Splunk Security Essentials
