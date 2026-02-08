# Specification Overview

The Synthetic Open Schema specification defines a vendor-neutral format for synthetic monitoring checks. This page provides an overview of the specification structure and how to navigate it.

---

## API Versions

Synthetic Open Schema uses semantic API versioning:

### `v1` — Core Checks (Stable)

Declarative checks with assertion-based validation:

- [HttpCheck](v1/http.md) — HTTP/HTTPS endpoint monitoring
- [TcpCheck](v1/tcp.md) — TCP port connectivity
- [DnsCheck](v1/dns.md) — DNS resolution validation
- [TlsCheck](v1/tls.md) — TLS/SSL certificate monitoring
- [DomainCheck](v1/domain.md) — Domain registration checks

**Status**: ✅ Stable (v1.0.0 released 2026-02-07)

[View v1 API documentation →](v1/_index.md)

### `browser/v1` — Browser Checks (Stable)

Programmatic browser automation checks:

- [LoadCheck](browser-v1/load_check.md) — Page load performance monitoring
- [ScriptedCheck](browser-v1/scripted_check.md) — Browser automation scripts

**Status**: ✅ Stable (v1.0.0 released 2026-02-07)

[View browser/v1 API documentation →](browser-v1/_index.md)

---

## Specification Structure

### Core Documents

| Document | Description |
|----------|-------------|
| [Base Model](v1/check.md) | Resource structure (`apiVersion`, `kind`, `metadata`, `spec`) |
| [Common Types](v1/common.md) | Shared fields, assertion types, operators, validation rules |

### Check Kind Specifications

Each check kind has its own specification document:

- **Fields**: Required and optional fields for the check
- **Assertions**: Supported assertion types for validation
- **Examples**: Complete YAML examples
- **Conformance**: Requirements for implementations

### Policy Documents

| Document | Description |
|----------|-------------|
| [Glossary](policy/glossary.md) | Terminology and definitions |
| [Versioning](policy/versioning.md) | API versioning strategy |
| [Compatibility](policy/compatibility.md) | Backward/forward compatibility guarantees |

---

## Resource Model

All checks follow a Kubernetes-style resource structure:

```yaml
apiVersion: v1              # or browser/v1
kind: HttpCheck             # Check type
metadata:
  name: my-check            # Unique identifier
  title: My Check           # Human-readable name (optional)
  labels:                   # Key-value pairs (optional)
    env: production
spec:
  # Check-specific configuration
  url: https://example.com
  interval: 1m
  checks:
    - type: statusCode
      value: 200
```

### Key Concepts

- **apiVersion**: Declares which API version the check uses
- **kind**: Specifies the check type (e.g., `HttpCheck`, `DnsCheck`)
- **metadata**: Information about the check (name, labels, etc.)
- **spec**: Check-specific configuration and assertions

[Learn more about the base model →](v1/check.md)

---

## Common Fields

All check kinds share these fields in their `spec`:

### Scheduling (Mutually Exclusive)

**Interval-based**:
```yaml
spec:
  interval: 1m  # Every minute
```

**Cron-based**:
```yaml
spec:
  cron: "*/5 * * * *"  # Every 5 minutes
```

### Execution

```yaml
spec:
  timeout: 30s              # Maximum execution time
  retries: 3                # Number of retry attempts
  retryDelay: 5s            # Delay between retries
  locations:                # Execution locations
    - us-east-1
    - eu-west-1
```

### Notifications

```yaml
spec:
  channels:                 # Notification channels
    - slack-alerts
    - pagerduty-critical
```

### Assertions

```yaml
spec:
  checks:                   # Success criteria
    - type: statusCode
      value: 200
    - type: responseTime
      operator: lessThan
      value: 500ms
```

[View complete common fields documentation →](v1/common.md)

---

## Assertion Types

Synthetic Open Schema supports five assertion types, each with specific operators and value formats:

- **Numeric** — Status codes, response times, counts (operators: equals, greaterThan, lessThan, etc.)
- **String** — Body content, headers, DNS records (operators: equals, contains, matches, etc.)
- **Boolean** — Binary states like connected, valid, enabled (operators: equals, notEquals)
- **Time-based** — Dates, expirations, durations (operators: same as numeric)
- **List** — DNS records, nameservers, arrays (operators: contains, containsAny, containsAll)

[View complete assertion documentation with examples →](v1/common.md#assertions)

---

## Conformance

An implementation claiming compatibility with Synthetic Open Schema MUST:

1. Implement the base Resource model as defined in [check.md](v1/check.md)
2. Support at least one check kind fully
3. Reject configurations that violate normative constraints
4. Clearly document which check kinds and features are supported
5. Handle scheduling (interval or cron) as specified
6. Evaluate assertions according to the specification

Implementations SHOULD:

- Support all stable check kinds
- Provide clear error messages for validation failures
- Support multiple execution locations
- Implement retry logic

[Learn more about conformance requirements →](v1/_index.md#conformance)

---

## Examples

Complete, tested YAML example configurations are available in the model repository:

**Core Checks (v1)**:

- [http-check.yaml](https://github.com/syntheticopenschema/model/blob/main/examples/http-check.yaml)
- [tcp-check.yaml](https://github.com/syntheticopenschema/model/blob/main/examples/tcp-check.yaml)
- [dns-check.yaml](https://github.com/syntheticopenschema/model/blob/main/examples/dns-check.yaml)
- [tls-check.yaml](https://github.com/syntheticopenschema/model/blob/main/examples/tls-check.yaml)
- [domain-check.yaml](https://github.com/syntheticopenschema/model/blob/main/examples/domain-check.yaml)

**Browser Checks (browser/v1)**:

- [load-check.yaml](https://github.com/syntheticopenschema/model/blob/main/examples/load-check.yaml)
- [scripted-check.yaml](https://github.com/syntheticopenschema/model/blob/main/examples/scripted-check.yaml)

[View all examples →](../examples.md)

---

## JSON Schema

JSON Schema definitions for validation and tooling integration are published at:

📦 https://github.com/syntheticopenschema/schemas

These schemas are generated from the reference Python implementation and updated with each release.

---

## Contributing

To propose changes to the specification:

1. Open an issue describing the proposed change
2. Submit a pull request with specification updates
3. Update examples and schemas accordingly
4. Ensure backward compatibility (or document breaking change)

[Read the contribution guidelines →](https://github.com/syntheticopenschema/spec/blob/main/CONTRIBUTING.md)

---

## Governance

The specification follows an open governance model:

- **Steward**: Ideatives Inc.
- **Maintainer**: @dmonroy
- **RFC Process**: New check types require 14-day community review
- **Decision Process**: Documented in [GOVERNANCE.md](https://github.com/syntheticopenschema/spec/blob/main/GOVERNANCE.md)

[Learn more about governance →](https://github.com/syntheticopenschema/spec/blob/main/GOVERNANCE.md)

---

## Support

- 💬 [GitHub Discussions](https://github.com/syntheticopenschema/spec/discussions)
- 🐛 [Report Issues](https://github.com/syntheticopenschema/spec/issues)
- 🔒 [Security Issues](https://github.com/syntheticopenschema/spec/blob/main/SECURITY.md)
