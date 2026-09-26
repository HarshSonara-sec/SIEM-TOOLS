# Splunk Data Models and Common Information Model (CIM)

## Data Models

A Splunk **data model** provides a structured way to organize related event data for analysis.

A data model can contain **datasets** arranged hierarchically. A broad parent dataset can contain more specific child datasets.

Conceptual example:

```text
Authentication
└── Successful Authentication
```

The exact hierarchy depends on the data model.

## Why Data Models Matter

Different systems may describe similar activity using different field names.

Example:

```text
source A → source_ip
source B → src
source C → client_ip
```

Normalization can map equivalent concepts to a common field so searches and detections can be reused.

## Common Information Model (CIM)

Splunk's **Common Information Model (CIM)** provides common field names, tags, and data models for different security and IT domains.

CIM helps normalize data at **search time** while preserving the original machine data.

Common domains include:

- Authentication
- Network Traffic
- Web
- Endpoint
- Malware
- Change
- Data Access

## Normalization Workflow

```text
Get Data In
   ↓
Identify Relevant Data Model
   ↓
Apply Appropriate Tags
   ↓
Normalize Field Names / Values
   ↓
Verify Fields
   ↓
Validate Against Data Model
```

Normalization can involve:

- field aliases
- field extractions
- lookups
- event types
- CIM-compliant tags

## Normalization vs Enrichment

### Normalization
Makes data consistent.

```text
different field names
        ↓
common field names
```

### Enrichment
Adds additional information.

```text
src_ip=8.8.8.8
        ↓
src_ip + Country + Region + City + coordinates
```

The `iplocation` command is an example of enrichment.

## Data Interpretation

After data is classified, normalized, modeled, and enriched, an analyst can interpret it.

Example:

```text
Failed login events
        ↓
Normalize authentication fields
        ↓
Enrich source IP
        ↓
Count by source/country
        ↓
Investigate unusual patterns
```

## SOC Relevance

Data models and CIM are important because SOC detections and dashboards often need to work across multiple vendors and data sources.

Key principle:

```text
Normalize → Enrich → Analyze → Detect / Investigate
```
