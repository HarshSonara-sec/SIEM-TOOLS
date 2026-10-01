# Wazuh — SIEM, Installation, Dashboard & SOC Operations

> **Purpose:** SOC Analyst reference for understanding Wazuh architecture, deployment, dashboard usage, agents, log analysis, alerts, rules, decoders, and investigation workflow.

---

# 1. What is Wazuh?

**Wazuh** is a free and open-source security platform providing **XDR and SIEM capabilities** for endpoints and cloud workloads.

Core capabilities include:

* Security monitoring
* Log analysis
* Threat detection
* Malware detection
* File Integrity Monitoring (FIM)
* Vulnerability detection
* Security Configuration Assessment (SCA)
* Regulatory compliance monitoring
* Incident response
* Threat hunting
* Endpoint inventory

Wazuh consists of a **Wazuh agent** and three central components:

```text
Wazuh Agent
     ↓
Wazuh Server
     ↓
Wazuh Indexer
     ↓
Wazuh Dashboard
```

---

# 2. Wazuh Architecture

## 2.1 Wazuh Agent

The agent is installed on monitored endpoints.

It collects security information such as:

* System logs
* Application logs
* Security events
* File changes
* System inventory
* Configuration information
* Vulnerability information

The agent sends collected data to the Wazuh server for analysis.

---

# 2.2 Wazuh Server

The Wazuh server is the central analysis and management component.

Responsibilities include:

* Receiving agent data
* Processing logs
* Decoding events
* Applying detection rules
* Generating alerts
* Managing agents
* Enriching security events
* Providing the Wazuh API
* Supporting incident-response actions

The server's analysis engine uses **decoders + rules** to process security events.

---

# 2.3 Wazuh Indexer

The Wazuh indexer is responsible for:

* Indexing security data
* Storing events
* Searching data
* Supporting analytics
* Providing data to the dashboard

The indexer stores security information as JSON documents and supports single-node or multi-node deployments.

---

# 2.4 Wazuh Dashboard

The Wazuh dashboard is the web interface used by analysts and administrators.

It provides:

* Security event visualization
* Alert investigation
* Agent management
* Threat hunting
* Malware information
* File Integrity Monitoring
* Vulnerability information
* System inventory
* Compliance information
* Custom dashboards
* Ruleset testing
* Platform management

---

# 3. Complete Data Flow

The basic Wazuh workflow is:

```text
Endpoint
   │
   │ Wazuh Agent
   ↓
Wazuh Server
   │
   ├── Decode logs
   ├── Apply rules
   ├── Generate alerts
   │
   ↓
Filebeat
   ↓
Wazuh Indexer
   ↓
Wazuh Dashboard
   ↓
SOC Analyst
```

The Wazuh documentation describes this architecture as agent → server → indexer → dashboard.

---

# 4. All-in-One Deployment

For a learning lab, Wazuh supports an **all-in-one deployment** where the central components are installed on the same machine:

```text
┌──────────────────────────────┐
│        Wazuh Server          │
│                              │
│  Wazuh Server                │
│  Wazuh Indexer               │
│  Wazuh Dashboard             │
└──────────────────────────────┘
             ↑
             │
        Wazuh Agents
```

Wazuh identifies all-in-one deployment as suitable for labs and smaller environments with limited numbers of monitored endpoints.

For learning SOC operations, this is a practical starting point.

---

# 5. Wazuh Installation

Wazuh provides multiple installation methods.

For a lab environment, the **installation assistant** is the simplest approach.

Official quickstart currently provides an all-in-one installation using:

```bash
curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh
sudo bash ./wazuh-install.sh -a
```

The installation assistant configures the central Wazuh components and displays the dashboard access information when installation finishes.

> **Important:** Always check the current Wazuh documentation before installing because package versions, commands, repositories, and requirements can change.

---

# 6. Wazuh Installation Architecture

The manual installation workflow is:

```text
1. Wazuh Indexer
        ↓
2. Wazuh Server
        ↓
3. Wazuh Dashboard
        ↓
4. Wazuh Agents
```

Wazuh documents both assisted and step-by-step installation methods.

---

# 7. Dashboard Installation

For a dedicated dashboard installation, the Wazuh documentation provides:

```bash
apt-get -y install wazuh-dashboard
```

The dashboard configuration includes settings such as:

```yaml
server.host: 0.0.0.0
server.port: 443
opensearch.hosts: https://localhost:9200
```

The exact configuration depends on whether the dashboard, indexer and server are installed on the same machine or separate nodes.

---

# 8. Starting the Dashboard

Systemd-based systems:

```bash
systemctl daemon-reload
systemctl enable wazuh-dashboard
systemctl start wazuh-dashboard
```

Check status:

```bash
systemctl status wazuh-dashboard
```

Restart:

```bash
systemctl restart wazuh-dashboard
```

Stop:

```bash
systemctl stop wazuh-dashboard
```

These commands are documented by Wazuh for managing the dashboard service.

---

# 9. Accessing the Dashboard

The default Wazuh dashboard uses HTTPS.

Typical access:

```text
https://<WAZUH_DASHBOARD_IP>
```

The default dashboard port is:

```text
443/TCP
```

The installation assistant displays the generated administrator credentials after installation.

### Security note

The initial installation may use certificates that are not trusted by the browser.

For a lab, the browser may show a certificate warning.

In production, use appropriate trusted certificates and secure credential management.

---

# 10. Main Dashboard Areas

Important areas to learn as a SOC analyst include:

### Security Events

Used to investigate:

* Alerts
* Events
* Rules
* Agents
* Source information
* Event fields

### Threat Hunting

Used to search and investigate security data.

### Agents

Used to:

* View agents
* Check agent status
* Deploy agents
* Organize agents
* Manage configurations

### Vulnerability Detection

Used to identify vulnerable software and systems.

### File Integrity Monitoring

Used to detect changes to monitored files.

### Security Configuration Assessment

Used to evaluate endpoint configuration against security policies.

### Inventory

Provides information about endpoint hardware/software.

### Server Management

Used for:

* Rules
* Decoders
* Configuration
* Logs
* Statistics

The Wazuh dashboard provides visualization, agent management, platform management and ruleset-testing capabilities.

---

# 11. Wazuh Agents

The Wazuh agent is installed on an endpoint and communicates with the Wazuh server.

Typical workflow:

```text
Install Agent
     ↓
Configure Server
     ↓
Enroll Agent
     ↓
Agent Connects
     ↓
Agent Sends Telemetry
     ↓
Server Analyzes Data
```

Agent enrollment establishes authorization and secure communication between the agent and server.

---

# 12. Important Wazuh Ports

Common default ports include:

| Port    | Protocol | Purpose                     |
| ------- | -------- | --------------------------- |
| `1514`  | TCP      | Agent communication         |
| `1515`  | TCP      | Agent enrollment            |
| `1516`  | TCP      | Wazuh cluster communication |
| `514`   | UDP      | Syslog collector            |
| `55000` | TCP      | Wazuh server API            |
| `9200`  | TCP      | Wazuh indexer               |

Port configuration can be changed depending on deployment requirements.

---

# 13. Log Processing

Wazuh's detection pipeline can be summarized as:

```text
Raw Log
   ↓
Collection
   ↓
Decoder
   ↓
Structured Fields
   ↓
Detection Rule
   ↓
Alert
   ↓
Indexer
   ↓
Dashboard
```

The Wazuh analysis engine uses decoders to parse logs and rules to determine whether an event should generate an alert.

---

# 14. Decoders

A **decoder** extracts useful information from raw log messages.

Example:

```text
Raw log
"User admin logged from 192.168.1.10"
```

Decoder extracts:

```text
user = admin
srcip = 192.168.1.10
```

The structured fields can then be used by detection rules.

Wazuh provides built-in decoders and allows custom decoders.

---

# 15. Rules

Rules determine whether decoded events represent security-relevant activity.

A simplified model:

```text
Decoded Event
      ↓
Rule Conditions
      ↓
Match?
 ┌────┴────┐
No        Yes
↓          ↓
Ignore    Alert
```

Rules can evaluate:

* Log patterns
* Field values
* Decoder output
* Regular expressions
* Previous rule matches
* Event conditions
* Security context

Rules can also associate events with MITRE ATT&CK techniques and compliance frameworks.

---

# 16. Rule Levels

Wazuh rules contain a **level** that represents the importance of the generated alert.

Example:

```xml
<rule id="5715" level="3">
```

The exact meaning of a severity level must be interpreted in the context of the rule and environment.

Do not automatically assume:

```text
High rule level = confirmed attack
```

A SOC analyst should investigate the underlying event and context.

---

# 17. Alerts

When a rule matches an event, Wazuh generates an alert.

Default alert files on the Wazuh server include:

```text
/var/ossec/logs/alerts/alerts.log
/var/ossec/logs/alerts/alerts.json
```

The JSON alert data is forwarded toward the Wazuh indexer and becomes available through the dashboard.

---

# 18. Important Wazuh Index Patterns

Common Wazuh index patterns include:

```text
wazuh-alerts-*
wazuh-archives-*
wazuh-monitoring-*
wazuh-statistics-*
wazuh-states-vulnerabilities-*
wazuh-states-inventory-hardware-*
```

Important distinction:

### `wazuh-alerts-*`

Contains alerts generated by Wazuh detection rules.

### `wazuh-archives-*`

Contains events sent to the Wazuh server.

### `wazuh-monitoring-*`

Contains agent monitoring information.

### `wazuh-states-vulnerabilities-*`

Contains vulnerability information.

These index patterns are useful to understand when investigating data at the indexer/search layer.

---

# 19. Wazuh Security Monitoring Capabilities

## File Integrity Monitoring — FIM

Detects changes to monitored files.

Useful for identifying:

* Unexpected file modification
* File creation
* File deletion
* Potential persistence
* Configuration changes

---

## Vulnerability Detection

Identifies vulnerable software/packages on monitored systems.

SOC analysts can use this information to:

* Identify vulnerable endpoints
* Prioritize investigation
* Correlate vulnerabilities with observed attacks

---

## Security Configuration Assessment — SCA

Evaluates system configuration against defined security policies.

Useful for:

* Hardening
* Compliance
* Configuration auditing

---

## Malware Detection

Wazuh can detect suspicious activity associated with malware and integrate endpoint security telemetry into investigations.

---

## Threat Hunting

Analysts can search collected security information for suspicious behavior that may not have generated a traditional alert.

---

# 20. Wazuh Ruleset Testing

Wazuh provides a **ruleset test tool** in the dashboard.

Its purpose is to test:

```text
Raw Log
   ↓
Decoder
   ↓
Rule Matching
   ↓
Result
```

This is especially useful when creating or troubleshooting custom rules and decoders.

---

# 21. Custom Rules & Decoders

Do not directly modify the built-in ruleset directory.

Wazuh states that the built-in rules and decoders under:

```text
/var/ossec/ruleset/
```

can be overwritten during upgrades.

Custom changes should instead be placed under:

```text
/var/ossec/etc/
```

Common locations include:

```text
/var/ossec/etc/rules/local_rules.xml
/var/ossec/etc/decoders/local_decoder.xml
```

---

# 22. Wazuh SOC Investigation Workflow

A basic analyst workflow:

```text
Alert
 ↓
Identify Rule
 ↓
Read Alert Details
 ↓
Identify Agent
 ↓
Identify User / Host / IP
 ↓
Inspect Raw Event
 ↓
Search Related Events
 ↓
Check Timeline
 ↓
Correlate Activity
 ↓
Determine Scope
 ↓
Check Threat Intelligence
 ↓
Determine Severity / Impact
 ↓
Respond or Escalate
 ↓
Document
```

---

# 23. Example Investigation

Suppose Wazuh generates an alert for suspicious authentication activity.

Start with:

```text
Alert
 ↓
Source IP
 ↓
Destination Host
 ↓
Username
 ↓
Authentication result
 ↓
Timestamp
```

Then investigate:

```text
Was the login successful?
        ↓
Which account was targeted?
        ↓
Which endpoint?
        ↓
Were there repeated attempts?
        ↓
Were other accounts targeted?
        ↓
Did activity occur after authentication?
        ↓
Were files/processes/network connections changed?
```

This converts an isolated alert into an investigation.

---

# 24. Useful Wazuh Commands

Check Wazuh manager:

```bash
systemctl status wazuh-manager
```

Start:

```bash
systemctl start wazuh-manager
```

Restart:

```bash
systemctl restart wazuh-manager
```

Check dashboard:

```bash
systemctl status wazuh-dashboard
```

Check indexer:

```bash
systemctl status wazuh-indexer
```

View alerts:

```bash
less /var/ossec/logs/alerts/alerts.log
```

View JSON alerts:

```bash
less /var/ossec/logs/alerts/alerts.json
```

Follow alerts in real time:

```bash
tail -f /var/ossec/logs/alerts/alerts.log
```

Follow JSON alerts:

```bash
tail -f /var/ossec/logs/alerts/alerts.json
```

> These commands are primarily useful for lab troubleshooting and understanding what is happening behind the dashboard.

---

# 25. Wazuh vs Traditional SIEM Workflow

For SOC learning, think of Wazuh as more than simply a log-search platform.

```text
Endpoint
   ↓
Agent
   ↓
Collection
   ↓
Analysis
   ├── Decoders
   ├── Rules
   └── Threat Intelligence
   ↓
Alerts
   ↓
Indexer
   ↓
Dashboard
   ↓
SOC Investigation
```

This integrated architecture gives Wazuh capabilities across:

* SIEM
* Endpoint monitoring
* Vulnerability management
* FIM
* Configuration assessment
* Threat detection
* Incident response

---

# 26. L1 SOC Analyst Focus

For a Junior/L1 SOC Analyst, prioritize these Wazuh areas:

### Must Know

* Wazuh architecture
* Agent/server/indexer/dashboard
* Dashboard navigation
* Security Events
* Alert investigation
* Agents
* Rule levels
* Rules
* Decoders
* Log analysis
* FIM
* Vulnerability detection
* Basic threat hunting
* Basic troubleshooting

### Should Know

* Agent enrollment
* Custom rules
* Custom decoders
* MITRE ATT&CK mappings
* Wazuh API basics
* Index patterns
* File locations
* Wazuh service management

### Later

* Wazuh clustering
* Advanced API automation
* Advanced custom detection engineering
* Large-scale deployment
* High availability
* Advanced integrations

---

# 27. Key Mental Model

Remember Wazuh using this simple model:

```text
AGENT
Collects data
   ↓
SERVER
Analyzes data
   ↓
DECODER
Understands the log
   ↓
RULE
Detects suspicious activity
   ↓
ALERT
Security event requiring investigation
   ↓
INDEXER
Stores/searches the data
   ↓
DASHBOARD
Visualizes the information
   ↓
SOC ANALYST
Investigates → Correlates → Responds
```

This is the most important Wazuh workflow to understand before moving into deeper hands-on investigations.

---

# 28. Official References

* Wazuh Documentation — Quickstart.
* Wazuh Architecture.
* Wazuh Components.
* Wazuh Dashboard.
* Wazuh Installation Guide.
* Wazuh Data Analysis / Rules & Decoders.
* Wazuh Agent Enrollment.
* Wazuh Indexer.
