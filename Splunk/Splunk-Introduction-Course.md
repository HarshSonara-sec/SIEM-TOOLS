# Splunk Introduction Course

## Overview

Notes from completing the **Introduction to Splunk** course on Splunk Education, expanded with official Splunk documentation and practical observations from the course screenshots.

Splunk is used to collect, index, search, analyze, visualize, and monitor machine-generated data such as logs and security events.

## Core Data Workflow

A useful way to understand the workflow shown in the course is:

1. **Data Classification** — identify what incoming data represents.
2. **Data Normalization** — make equivalent fields and values consistent across sources.
3. **Data Models** — organize normalized data into structured datasets for analysis.
4. **Data Enrichment** — add useful context to events, such as IP geolocation or lookup data.
5. **Data Interpretation** — analyze the resulting information to identify patterns, activity, and security events.

The course material also introduces **hierarchically structured datasets**: data models can contain datasets arranged from broader parent datasets to more specific child datasets.

## Important Splunk Concepts

### Event
A single piece of machine-generated data. Example: one failed SSH login.

### Field
A name/value pair extracted from an event.

```text
src_ip=23.158.56.x
host=www1
sourcetype=linux_secure
```

### Index
A logical location where Splunk stores indexed event data.

```text
index=security
```

### Sourcetype
Describes the type/format of incoming data and helps Splunk interpret events.

```text
sourcetype=linux_secure
```

### Search Processing Language (SPL)
Splunk's search language is used to retrieve, filter, transform, aggregate, and visualize data.

```text
<initial search> | <command> | <command>
```

The pipe `|` passes the results of one search operation to the next command.

## Example Security Search

```spl
index=security sourcetype=linux_secure host!=mail "Failed password" src_ip=*
```

This searches the `security` index for Linux security events containing failed-password activity, excludes the `mail` host, and requires a `src_ip` field.

## Key Learning

For SOC work, Splunk is not only about finding a matching string in logs. A useful workflow is:

```text
Raw Events
   ↓
Fields
   ↓
Classification / Normalization
   ↓
Data Models
   ↓
Enrichment
   ↓
Search + Statistics
   ↓
Visualization / Detection / Investigation
```

## SOC Relevance

A SOC analyst can use Splunk to:

- investigate authentication failures
- identify suspicious source IPs
- correlate activity across hosts
- enrich events with additional context
- summarize large event sets
- visualize activity
- build searches that support detections and investigations
