# Synthetic Open Schema

<div class="grid cards" markdown>

-   :material-check-circle:{ .lg .middle } __Portable__

    ---

    Write checks once, run on any compatible runner. No vendor lock-in.

-   :material-application-brackets:{ .lg .middle } __Declarative__

    ---

    YAML-based, Kubernetes-style configuration. Easy to read and write.

-   :material-shield-check:{ .lg .middle } __Production Ready__

    ---

    Stable v1.0.0 specification. Battle-tested in production environments.

-   :material-open-source-initiative:{ .lg .middle } __Open Source__

    ---

    Apache 2.0 licensed. Community-driven, vendor-neutral standard.

</div>

---

## What is Synthetic Open Schema?

Synthetic Open Schema (SOS) is an **open specification** that defines a vendor-neutral format for synthetic monitoring checks. It enables you to define your monitoring checks once and run them anywhere.

[Get Started](getting-started.md){ .md-button .md-button--primary }
[Read Specification](specification/index.md){ .md-button }
[View on GitHub](https://github.com/syntheticopenschema/spec){ .md-button }

---

## Quick Example

```yaml title="api-health-check.yaml"
apiVersion: v1
kind: HttpCheck
metadata:
  name: api-health-check
  title: API Health Check
spec:
  url: https://api.example.com/health
  method: GET
  interval: 1m
  timeout: 30s
  checks:
    - type: statusCode
      value: 200
    - type: responseTime
      operator: lessThan
      value: 500ms
```

[See more examples →](examples.md)

---

## Supported Check Types

### Core Checks (`apiVersion: v1`)

<div class="grid cards" markdown>

-   **HttpCheck**

    ---

    Monitor HTTP/HTTPS endpoints with status codes, headers, response times, and body validation.

    [:octicons-arrow-right-24: Learn more](specification/v1/http.md)

-   **TcpCheck**

    ---

    Validate TCP port connectivity and optional response content.

    [:octicons-arrow-right-24: Learn more](specification/v1/tcp.md)

-   **DnsCheck**

    ---

    Verify DNS resolution for A, AAAA, CNAME, MX, TXT, NS, and PTR records.

    [:octicons-arrow-right-24: Learn more](specification/v1/dns.md)

-   **TlsCheck**

    ---

    Monitor TLS/SSL certificates for expiration, validity, and custom CAs.

    [:octicons-arrow-right-24: Learn more](specification/v1/tls.md)

-   **DomainCheck**

    ---

    Track domain registration, expiration, nameservers, and WHOIS data.

    [:octicons-arrow-right-24: Learn more](specification/v1/domain.md)

</div>

### Browser Checks (`apiVersion: browser/v1`)

<div class="grid cards" markdown>

-   **LoadCheck**

    ---

    Monitor page load performance with real browsers. Track FCP, LCP, and network timing.

    [:octicons-arrow-right-24: Learn more](specification/browser-v1/load_check.md)

-   **ScriptedCheck**

    ---

    Execute custom browser automation scripts with Playwright-based scripting.

    [:octicons-arrow-right-24: Learn more](specification/browser-v1/scripted_check.md)

</div>

---

## Implementations

### Official

<div class="grid" markdown>

```bash title="Python Model"
pip install synthetic-open-schema-model
```

```bash title="Python Runner"
pip install synthetic-open-schema-runner
```

</div>

[:octicons-arrow-right-24: View all implementations](implementations.md)

---

## Open Source

Licensed under **Apache License 2.0**. Stewarded by **Ideatives Inc.**

[:octicons-arrow-right-24: Contributing Guide](https://github.com/syntheticopenschema/spec/blob/main/CONTRIBUTING.md)
[:octicons-arrow-right-24: Governance](https://github.com/syntheticopenschema/spec/blob/main/GOVERNANCE.md)
[:octicons-arrow-right-24: Security](https://github.com/syntheticopenschema/spec/blob/main/SECURITY.md)
