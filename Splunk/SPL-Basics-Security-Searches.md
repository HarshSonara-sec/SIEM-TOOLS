# Splunk SPL Basics for Security Searches

## SPL Pipeline

Splunk searches commonly use the pipe operator:

```spl
search | command | command
```

Each command receives the previous command's results.

## Search Example

```spl
index=security sourcetype=linux_secure host!=mail "Failed password" src_ip=*
```

### Breakdown

| Part | Purpose |
|---|---|
| `index=security` | Search the security index |
| `sourcetype=linux_secure` | Limit results to Linux security data |
| `host!=mail` | Exclude events from the `mail` host |
| `"Failed password"` | Search for the exact phrase |
| `src_ip=*` | Require a source IP field |

## Enriching an IP Address

```spl
... | iplocation src_ip
```

`iplocation` looks up an IP address and adds location-related fields such as:

- `City`
- `Country`
- `Region`
- `lat`
- `lon`

This is useful when investigating the geographic origin of network or authentication activity.

## Example

```spl
index=security sourcetype=linux_secure host!=mail "Failed password" src_ip=*
| iplocation src_ip
```

## Aggregating Results

```spl
... | stats count by Country
```

Produces a tabular summary.

For geographic visualization:

```spl
... | geostats count by Country
```

`geostats` generates geographic statistics suitable for map visualization.

## Full Investigation Example

```spl
index=security sourcetype=linux_secure host!=mail "Failed password" src_ip=*
| iplocation src_ip
| stats count by Country
```

Map-oriented version:

```spl
index=security sourcetype=linux_secure host!=mail "Failed password" src_ip=*
| iplocation src_ip
| geostats count by Country
```

## Analyst Thinking

```text
Filter
  ↓
Enrich
  ↓
Aggregate
  ↓
Visualize
  ↓
Investigate
```

Do not treat geographic location alone as proof that an IP is malicious. It is contextual information that should be combined with authentication patterns, timestamps, usernames, destination systems, threat intelligence, and other evidence.

## Useful Commands

| Command | Purpose |
|---|---|
| `search` | Filter/search events |
| `iplocation` | Add geographic information for IP addresses |
| `stats` | Calculate aggregate statistics |
| `geostats` | Aggregate geographic data for map visualization |
| `table` | Display selected fields |
| `lookup` | Add information from lookup data |
