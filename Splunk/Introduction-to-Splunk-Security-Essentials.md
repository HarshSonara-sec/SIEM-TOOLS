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
