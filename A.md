# Kubernetes Log Streamer: Standardized Error Classification, Dynamic Popovers, and Pega/OPS Rules

## Objective

Implement a production\-ready, configuration\-driven error classification and detail\-popover framework in the existing Kubernetes Log Streamer\.

The existing log streamer already has:

- `logviewer_rules.yml`
- a YAML rule engine
- automatic insight classification in `analytics.py`
- `rule_matches` attached to log entries
- an `insight` object attached to log entries
- frontend rendering in `KubernetesLogViewer.tsx`
- existing error/category badges
- existing search/highlight behavior

Do not replace the existing architecture unnecessarily\. First inspect the repository and understand the current implementation, data flow, API payloads, frontend components, styling system, and tests\. Then make the smallest clean architectural change that achieves the requirements below\.

The Kubernetes Log Streamer is intended to become the single log viewer for Pega, internally developed applications, and third\-party/vendor applications\. Therefore, do NOT create a Pega\-specific implementation in Python or React\. Pega must be one configuration definition inside a generic framework\.

---

## 1\. Preserve existing behavior

Before modifying anything:

1. Audit the existing:
   - `logviewer_rules.yml`
   - rule loader/evaluator
   - `analytics.py`
   - session/log entry models
   - API/session/search/refresh payloads
   - `KubernetesLogViewer.tsx`
   - existing badge components
   - existing popover/tooltip components and theme styles
   - existing tests
2. Preserve:
   - existing generic error detection
   - existing logging\-level badges
   - existing rule matching
   - existing search/highlight behavior
   - existing session lifecycle
   - existing Kubernetes log streaming behavior
   - existing API contracts unless an additive field is required
   - backward compatibility with rules that only contain the existing fields

Do not remove the current generic classifier\.

Do not introduce an external LLM/API dependency for per\-line log summarization\. The dynamic explanation must be derived locally from the actual matched log occurrence plus configured rule metadata\.

---

# 2\. Configuration architecture

Keep the configuration\-driven approach\.

Use a versioned YAML structure supporting multiple definitions:

```yaml
version: 2

definitions:
  - id: generic
    vendor: "*"
    application: "*"
    rules:
      ...

  - id: pega
    vendor: pega
    application: "*"
    rules:
      ...

  - id: future-application
    vendor: internal
    application: future-application
    rules:
      ...
```

### Meaning of the definitions

`generic` contains patterns applicable across applications/vendors\.

`pega` contains Pega\-specific signatures\.

Future vendors/applications can be added by YAML without changing the rule engine\.

The `id` is an internal stable identifier\. It must NOT be displayed directly as the user\-facing badge\.

Use:

- `id` for machine identity
- `label` for the visible badge
- `category` for normalized classification
- `description` for stable human\-readable explanation
- `severity` for badge severity/color
- `scope` for vendor/application/log\-type applicability
- optional occurrence\-detail metadata for contextual explanation

Rules without scope must remain globally applicable for backward compatibility\.

---

# 3\. Exact Pega and OPS signatures

Do NOT replace the verified Pega/OPS signatures with a broad `PEGA-[0-9]+` rule\.

The following exact signatures have already been validated for this project:

|Code    |Meaning / label      |Category             |Suggested severity|
|--------|---------------------|---------------------|------------------|
|PEGA0001|HTTP Slow            |latency              |WARN              |
|PEGA0002|DB Commit Slow       |database/latency     |WARN              |
|PEGA0004|DB Data Large        |database             |WARN              |
|PEGA0005|DB Query Slow        |database/latency     |WARN              |
|PEGA0026|DB Connection Wait   |database/availability|WARN              |
|OPS0007 |DB Connection Failure|database/connectivity|ERROR             |
|OPS0020 |DB Disconnected      |database/connectivity|ERROR             |

Match these exact codes as signatures\.

Do not invent additional Pega or OPS codes\.

Do not silently broaden these patterns\.

The visible badge should use the human\-readable `label`, not the raw code\.

The raw code must remain available in the rule/occurrence metadata and popover\.

Example:

```yaml
- id: pega-0005
  label: DB Query Slow
  pattern: '\bPEGA0005\b'
```

not:

```yaml
pattern: 'PEGA-[0-9]+'
```

---

# 4\. Generic rules

Preserve the existing generic rules such as:

- Timeout
- Exception
- Database Error
- Out of Memory
- connection failures
- DNS/unknown host
- authentication
- authorization
- HTTP
- memory
- file
- null pointer
- SSL/TLS
- generic Error fallback

Do not allow the new Pega rules to break generic classification\.

Where both a configured rule and automatic classification match, preserve both pieces of information rather than destroying one\.

Avoid duplicate visible badges where practical\. If the current architecture can produce both a configured badge and automatic insight badge for the same condition, improve the rendering so the UI does not become visually noisy, while preserving the underlying data\.

Do not make this change at the expense of existing functionality\.

---

# 5\. Severity and visual treatment

The visible classification badge must be severity\-aware\.

Use the application’s existing severity styling if one already exists\.

If the current UI already uses the appropriate severity colors, reuse those components/styles rather than introducing a second color system\.

At minimum:

- INFO → existing INFO styling
- WARN → existing WARN styling
- ERROR → existing ERROR styling
- FATAL → existing FATAL styling

Do not hard\-code unrelated colors into the new implementation if the application already has a theme/token system\.

The badge text should be human\-readable:

```text
DB Query Slow
```

rather than:

```text
PEGA0005
```

The raw code can appear inside the popover\.

---

# 6\. Standard popover

When the user hovers over or clicks the error/classification badge, display the same standard popover style already used elsewhere in the utility\.

Requirements:

- black/dark background
- rounded corners
- existing utility spacing
- existing typography
- existing shadow/border treatment
- visually polished
- consistent with the rest of the application
- do not create an unrelated new popover design

First locate the existing popover/tooltip component and reuse it if possible\.

The popover should be large enough to present useful diagnostic information but must not become a giant log dump\.

---

# 7\. Popover content model

The popover must combine TWO kinds of information\.

## A\. Static rule information

This comes from YAML and remains consistent for every occurrence of the same rule\.

Example:

```text
DB Query Slow

Category
Database / Latency

Severity
WARN

What this means
The Pega database query exceeded the expected execution time.

Detected signature
PEGA0005
```

This is the stable explanation of the error class\.

## B\. Dynamic occurrence information

This must come from the actual log occurrence\.

It must NOT be identical for every occurrence\.

For example, two PEGA0005 log entries may have different:

- SQL/query information
- duration
- database host
- table
- request/thread information
- timestamps
- endpoints
- identifiers
- surrounding context

The popover should extract and present useful occurrence\-specific information where it is actually available\.

Example:

```text
Occurrence details

Detected text
PEGA0005 ... query exceeded threshold ...

Duration
8.4 seconds

Database
db-prod-02

Request
ABC123

Context
The query execution exceeded the configured threshold while processing the request.
```

Do not fabricate values\.

If a field cannot be extracted from the actual log, omit it\.

---

# 8\. Dynamic summary

Add a concise, deterministic summary derived from the actual matched log occurrence\.

This is NOT an LLM\-generated summary\.

The summary should use:

1. the matched rule
2. the actual matched text
3. extracted occurrence fields
4. nearby log context when available

Example:

```text
Summary

Pega reported a slow database query. The occurrence indicates
a query duration of 8.4 seconds against db-prod-02.
```

If the log does not contain enough information:

```text
Summary

Pega reported a DB Query Slow condition. Additional occurrence
details were not present in the matched log entry.
```

Never invent causes, values, endpoints, IDs, durations, or remediation steps\.

---

# 9\. Occurrence extraction must be configuration\-driven

Extend the YAML schema so a rule can optionally define occurrence extraction\.

Example conceptual structure:

```yaml
- id: pega-0005
  label: DB Query Slow
  category: database
  severity: WARN
  pattern: '\bPEGA0005\b'
  description: "Pega reported a slow database query."
  details:
    summary_template: "Pega reported a slow database query."
    extract:
      - id: duration
        label: Duration
        pattern: '...'
      - id: database
        label: Database
        pattern: '...'
```

The exact extraction patterns must be based on the actual log formats found in the repository/test fixtures\. Do not invent application\-specific log formats merely to populate the YAML\.

The rule engine should expose:

- matched text
- match location/range where available
- extracted fields
- surrounding context
- rule metadata

to the frontend\.

---

# 10\. Data contract

Extend the existing rule\-match model additively\.

A rule match should be capable of representing something conceptually like:

```json
{
  "id": "pega-0005",
  "name": "DB Query Slow",
  "label": "DB Query Slow",
  "category": "database",
  "severity": "WARN",
  "description": "Pega reported a slow database query.",
  "matched_text": "PEGA0005 ...",
  "details": {
    "duration": "8.4 seconds",
    "database": "db-prod-02"
  },
  "summary": "Pega reported a slow database query. The occurrence indicates a query duration of 8.4 seconds against db-prod-02."
}
```

Keep the existing fields required by current consumers\.

If the current model already has equivalent fields, reuse them instead of duplicating them\.

---

# 11\. Insight\-engine future compatibility

Do NOT implement full Insight Engine integration in this change\.

However, structure the new data so it can be consumed later\.

The normalized event should retain:

- stable rule ID
- vendor
- application
- category
- severity
- label
- timestamp
- pod/container
- matched text
- extracted details
- normalized summary/fingerprint
- raw occurrence reference where appropriate

This should make it possible for the future Insight Engine to aggregate:

```text
PEGA0005 occurrences
↓
frequency
↓
affected applications/pods
↓
time trends
↓
recurring fingerprint
↓
historical insight
```

Do not add database persistence solely for this feature\.

Do not create a second storage system\.

The current log streamer remains session\-oriented\.

---

# 12\. Fingerprinting

Where the existing analytics system already creates normalized fingerprints, preserve and reuse that mechanism\.

For dynamic occurrence data, avoid using highly volatile values directly as the fingerprint\.

For example:

```text
PEGA0005 query on db-prod-01 duration 8.4s
```

and:

```text
PEGA0005 query on db-prod-02 duration 12.1s
```

should be capable of resolving to the same logical error fingerprint when the underlying condition is the same\.

Do not regress the existing fingerprint behavior\.

---

# 13\. Application/vendor scope

The implementation must support future definitions such as:

```yaml
- id: internal-payment-service
  vendor: internal
  application: payment-service
```

and:

```yaml
- id: vendor-x
  vendor: vendor-x
  application: "*"
```

The existing K8s session metadata should be used where available to determine application/vendor scope\.

Do not require the user to configure Pega\-specific logic in Python code\.

---

# 14\. Frontend behavior

For every matching log entry:

```text
normal log line
    +
severity badge
    +
classification badge
```

The classification badge should be hoverable/clickable\.

Popover:

```text
┌─────────────────────────────────────┐
│ DB Query Slow                  WARN │
│                                     │
│ Database / Latency                  │
│                                     │
│ What this means                    │
│ Pega reported a slow database      │
│ query.                             │
│                                     │
│ Detected signature                 │
│ PEGA0005                           │
│                                     │
│ Occurrence                         │
│ Duration: 8.4 seconds              │
│ Database: db-prod-02               │
│                                     │
│ Summary                            │
│ Pega reported a slow database      │
│ query against db-prod-02.         │
└─────────────────────────────────────┘
```

Use the existing popover component and design system\.

Do not show empty sections\.

Do not duplicate the entire raw log message unless the existing UX specifically requires it\.

Allow long values to wrap safely\.

Do not let the popover break the log viewer layout or obscure essential controls\.

---

# 15\. Search behavior

Existing search/highlight behavior must remain intact\.

If a rule has `matched_text`/highlight ranges, preserve those ranges\.

Do not change the semantics of search merely to support popovers\.

---

# 16\. API/WebSocket compatibility

The existing Kubernetes Log Streamer currently uses its established HTTP/session architecture\.

Do not reintroduce the old Pega WebSocket architecture\.

Do not create a separate Pega API\.

Add the new classification details to the existing K8s log entry payload in a backward\-compatible manner\.

Existing clients that ignore the new fields must continue to work\.

---

# 17\. Testing

Add/update tests covering at least:

### Rule matching

- PEGA0001
- PEGA0002
- PEGA0004
- PEGA0005
- PEGA0026
- OPS0007
- OPS0020

Verify exact\-code matching\.

Verify that partial/incorrect codes do not accidentally match\.

### Generic rules

Verify existing generic rules continue to work\.

### Scope

Verify generic rules apply globally\.

Verify Pega rules apply only to Pega\-scoped entries where scope information is available\.

### Multiple matches

Verify a log containing both a Pega signature and a generic condition does not crash and does not lose metadata\.

### Dynamic details

Use representative real/test log messages and verify that occurrence\-specific fields differ when the underlying log differs\.

Verify missing fields are omitted rather than fabricated\.

### Popover

Verify:

- correct label
- correct severity styling
- static explanation
- exact code
- dynamic occurrence details
- dynamic summary
- no empty sections
- long values wrap correctly

### Backward compatibility

Existing logs with no configured metadata must render exactly as before\.

Existing search behavior must continue to work\.

Existing API/session tests must pass\.

---

# 18\. Implementation discipline

Do not blindly implement the proposed schema if the current repository already has an equivalent abstraction\.

First inspect and reuse existing:

- models
- serializers
- rule loaders
- classifiers
- popovers
- theme tokens
- UI components
- test helpers

Prefer additive evolution over rewriting\.

Avoid duplicate classification engines\.

There should ultimately be one coherent pipeline:

```text
raw Kubernetes log
        ↓
parse log entry
        ↓
configured rule evaluation
        ↓
automatic classification
        ↓
occurrence extraction
        ↓
normalized log classification
        ↓
API/session payload
        ↓
badge
        ↓
standard popover
```

The rule configuration should remain the primary place where application/vendor\-specific behavior is defined\.

---

# 19\. Required final report

After implementation, provide a concise implementation report containing:

1. Files changed
2. Existing architecture discovered
3. New YAML schema
4. Exact Pega/OPS rules implemented
5. How occurrence\-specific details are extracted
6. How the dynamic summary is generated
7. How the popover reuses existing styling
8. API/model changes
9. Insight Engine future compatibility
10. Tests added/updated
11. Tests executed and results
12. Any assumptions or log formats that could not be safely inferred

Do not claim tests passed unless they were actually executed\.
