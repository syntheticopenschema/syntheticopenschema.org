# Getting Started

Welcome to Synthetic Open Schema! This guide will help you create and run your first synthetic check in under 5 minutes.

---

## What You'll Build

A simple HTTP endpoint monitor that:

- Checks if an API returns HTTP 200
- Validates response time is under 1 second
- Runs every 5 minutes

---

## Prerequisites

- Basic understanding of YAML
- A text editor
- (Optional) Python 3.11+ to run checks locally

---

## Step 1: Create Your First Check

Create a file named `my-first-check.yaml`:

```yaml title="my-first-check.yaml"
apiVersion: v1
kind: HttpCheck
metadata:
  name: my-api-check
  title: My First API Check
  labels:
    environment: production
    team: platform
spec:
  url: https://httpbin.org/status/200
  method: GET
  interval: 5m
  timeout: 30s
  checks:
    - type: statusCode
      value: 200
    - type: responseTime
      operator: lessThan
      value: 1s
```

---

## Step 2: Understand the Structure

Every Synthetic Open Schema check follows this structure:

### `apiVersion`
Specifies which API version to use:
- `v1` — Core checks (HTTP, TCP, DNS, TLS, Domain)
- `browser/v1` — Browser automation checks (Load, Scripted)

### `kind`
The type of check:
- `HttpCheck`, `TcpCheck`, `DnsCheck`, `TlsCheck`, `DomainCheck`
- `LoadCheck`, `ScriptedCheck`

### `metadata`
Information about the check:
- `name` — Unique identifier (DNS-subdomain format)
- `title` — Human-readable name (optional)
- `labels` — Key-value pairs for organization (optional)

### `spec`
Check-specific configuration:
- **Scheduling**: `interval` (e.g., `1m`, `5s`) or `cron` expression
- **Execution**: `timeout`, `retries`, `locations`
- **Notifications**: `channels` for alerting
- **Assertions**: `checks` array defining success criteria

---

## Step 3: Run Your Check

### Option A: Using Python Runner

Install the runner:

```bash
pip install synthetic-open-schema-runner
```

Run your check:

```bash
sos-runner my-first-check.yaml
```

Expected output:

```
✓ my-api-check: PASSED
  ✓ statusCode: 200 equals 200
  ✓ responseTime: 245ms lessThan 1s

Duration: 245ms
```

---

## Step 4: Explore More Check Types

Synthetic Open Schema supports 7 check types across two API versions:

**Core Checks (v1):**
- **TcpCheck** — Monitor TCP port connectivity
- **DnsCheck** — Verify DNS resolution for any record type
- **TlsCheck** — Monitor certificate expiration and validity
- **DomainCheck** — Track domain registration and WHOIS data

**Browser Checks (browser/v1):**
- **LoadCheck** — Measure real browser page load performance
- **ScriptedCheck** — Execute custom Playwright automation scripts

[View complete examples for all check types →](examples.md)

---

## Step 5: Add Assertions

Assertions define what "success" means for your check. Different check types support different assertion types:

### Numeric Assertions

```yaml
checks:
  - type: statusCode
    value: 200                    # equals
  - type: responseTime
    operator: lessThan
    value: 500ms
  - type: responseTime
    operator: greaterThanOrEqual
    value: 100ms
```

Operators: `equals`, `notEquals`, `greaterThan`, `lessThan`, `greaterThanOrEqual`, `lessThanOrEqual`

### String Assertions

```yaml
checks:
  - type: body
    operator: contains
    value: "success"
  - type: header
    key: Content-Type
    operator: matches
    value: "^application/json"
```

Operators: `equals`, `notEquals`, `contains`, `notContains`, `matches` (regex)

### Boolean Assertions

```yaml
checks:
  - type: dnssecEnabled
    value: true
  - type: connect
    value: true
```

Operators: `equals`, `notEquals`

[View all assertion types →](specification/v1/common.md#assertions)

---

## Step 6: Schedule Your Checks

### Interval-Based Scheduling

```yaml
spec:
  interval: 1m      # Every minute
  interval: 30s     # Every 30 seconds
  interval: 1h      # Every hour
  interval: 1d      # Every day
```

### Cron-Based Scheduling

```yaml
spec:
  cron: "0 * * * *"        # Every hour
  cron: "*/5 * * * *"      # Every 5 minutes
  cron: "0 0 * * *"        # Daily at midnight
  cron: "0 9 * * MON-FRI"  # Weekdays at 9am
```

!!! note
    `interval` and `cron` are mutually exclusive. Use one or the other, not both.

---

## Next Steps

### Learn the Specification

- [Read the complete v1 specification](specification/v1/_index.md)
- [Understand common types and fields](specification/v1/common.md)
- [Explore browser checks](specification/browser-v1/_index.md)

### See More Examples

- [Browse example configurations](examples.md)
- [View the model repository examples](https://github.com/syntheticopenschema/model/tree/main/examples)

### Build or Integrate

- [View existing implementations](implementations.md)

### Join the Community

- [Contribute to the specification](https://github.com/syntheticopenschema/spec/blob/main/CONTRIBUTING.md)
- [Read the governance model](https://github.com/syntheticopenschema/spec/blob/main/GOVERNANCE.md)
- [Report security issues](https://github.com/syntheticopenschema/spec/blob/main/SECURITY.md)

---

## Need Help?

- 💬 [GitHub Discussions](https://github.com/syntheticopenschema/spec/discussions)
- 🐛 [Report Issues](https://github.com/syntheticopenschema/spec/issues)
- 📖 [Read the Spec](specification/index.md)

---

**Ready to monitor?** Start writing your checks! 🚀
