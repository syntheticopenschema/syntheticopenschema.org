# Examples

Complete, tested YAML example configurations for all check types. All examples are validated against the reference Python implementation.

---

## Core Checks (`apiVersion: v1`)

### HttpCheck — HTTP/HTTPS Endpoint Monitoring

```yaml title="http-check.yaml"
apiVersion: v1
kind: HttpCheck
metadata:
  name: api-health-check
  title: API Health Check
  labels:
    environment: production
    service: api
spec:
  url: https://api.example.com/health
  method: GET
  interval: 1m
  timeout: 30s
  headers:
    User-Agent: SyntheticMonitor/1.0
    Accept: application/json
  checks:
    - type: statusCode
      value: 200
    - type: responseTime
      operator: lessThan
      value: 500ms
    - type: body
      operator: contains
      value: '"status":"ok"'
    - type: header
      key: Content-Type
      operator: contains
      value: application/json
```

[View full HttpCheck specification →](specification/v1/http.md)

---

### TcpCheck — TCP Port Connectivity

```yaml title="tcp-check.yaml"
apiVersion: v1
kind: TcpCheck
metadata:
  name: database-connectivity
  title: Database TCP Check
spec:
  host: db.example.com
  port: 5432
  interval: 5m
  timeout: 10s
  retries: 3
  retryDelay: 5s
  checks:
    - type: connect
      value: true
    - type: responseTime
      operator: lessThan
      value: 100ms
```

[View full TcpCheck specification →](specification/v1/tcp.md)

---

### DnsCheck — DNS Resolution

```yaml title="dns-check.yaml"
apiVersion: v1
kind: DnsCheck
metadata:
  name: domain-resolution
  title: DNS A Record Check
spec:
  hostname: example.com
  recordType: A
  interval: 10m
  nameserver: 8.8.8.8  # Optional: use specific DNS server
  checks:
    - type: addresses
      operator: contains
      value: "93.184.216.34"
    - type: responseTime
      operator: lessThan
      value: 100ms
```

[View full DnsCheck specification →](specification/v1/dns.md)

---

### TlsCheck — TLS/SSL Certificate Monitoring

```yaml title="tls-check.yaml"
apiVersion: v1
kind: TlsCheck
metadata:
  name: api-certificate
  title: API TLS Certificate Check
spec:
  host: api.example.com
  port: 443
  interval: 6h
  timeout: 30s
  checks:
    - type: validUntil
      operator: greaterThan
      value: 30d
    - type: valid
      value: true
    - type: issuer
      operator: contains
      value: "Let's Encrypt"
```

**SMTP with STARTTLS**:

```yaml title="tls-smtp.yaml"
apiVersion: v1
kind: TlsCheck
metadata:
  name: smtp-tls-check
spec:
  host: smtp.example.com
  port: 587
  starttls: smtp  # Use STARTTLS protocol
  interval: 1h
  checks:
    - type: valid
      value: true
    - type: validUntil
      operator: greaterThan
      value: 7d
```

[View full TlsCheck specification →](specification/v1/tls.md)

---

### DomainCheck — Domain Registration Monitoring

```yaml title="domain-check.yaml"
apiVersion: v1
kind: DomainCheck
metadata:
  name: domain-expiration
  title: Domain Registration Check
spec:
  domain: example.com
  interval: 1d
  timeout: 30s
  checks:
    - type: expiresAt
      operator: greaterThan
      value: 60d
    - type: nameservers
      operator: containsAll
      values:
        - ns1.example.com
        - ns2.example.com
    - type: dnssecEnabled
      value: true
    - type: registrar
      operator: equals
      value: "Example Registrar Inc."
```

[View full DomainCheck specification →](specification/v1/domain.md)

---

## Browser Checks (`apiVersion: browser/v1`)

### LoadCheck — Page Load Performance

```yaml title="load-check.yaml"
apiVersion: browser/v1
kind: LoadCheck
metadata:
  name: homepage-load
  title: Homepage Load Performance
spec:
  url: https://example.com
  interval: 5m
  timeout: 60s
  browser:
    browserType: chromium
    viewport:
      width: 1920
      height: 1080
    userAgent: "Mozilla/5.0 (custom)"
  waitUntil: networkidle
  screenshot: true
  checks:
    - type: firstContentfulPaint
      operator: lessThan
      value: 2s
    - type: largestContentfulPaint
      operator: lessThan
      value: 3s
    - type: pageLoadTime
      operator: lessThan
      value: 5s
```

[View full LoadCheck specification →](specification/browser-v1/load_check.md)

---

### ScriptedCheck — Browser Automation

```yaml title="scripted-check.yaml"
apiVersion: browser/v1
kind: ScriptedCheck
metadata:
  name: login-flow
  title: User Login Flow Test
spec:
  url: https://app.example.com/login
  interval: 10m
  timeout: 120s
  browser:
    browserType: chromium
  script: |
    // Fill login form
    await page.fill('#username', 'testuser');
    await page.fill('#password', 'testpass');

    // Click login button
    await page.click('button[type="submit"]');

    // Wait for dashboard
    await page.waitForSelector('.dashboard');

    // Verify logged in
    const username = await page.textContent('.user-name');
    if (username !== 'Test User') {
      throw new Error('Login failed');
    }
  screenshot: true
  checks:
    - type: scriptSuccess
      value: true
    - type: duration
      operator: lessThan
      value: 10s
```

**With External Script File**:

```yaml title="scripted-check-with-file.yaml"
apiVersion: browser/v1
kind: ScriptedCheck
metadata:
  name: checkout-flow
spec:
  url: https://shop.example.com
  interval: 15m
  scriptFile: scripts/checkout-test.js
  scriptParams:
    productId: "12345"
    quantity: "2"
  checks:
    - type: scriptSuccess
      value: true
```

[View full ScriptedCheck specification →](specification/browser-v1/scripted_check.md)

---

## Advanced Examples

### Multi-Location HTTP Check

```yaml
apiVersion: v1
kind: HttpCheck
metadata:
  name: global-api-check
spec:
  url: https://api.example.com/v1/status
  interval: 1m
  locations:
    - us-east-1
    - eu-west-1
    - ap-southeast-1
  checks:
    - type: statusCode
      value: 200
    - type: responseTime
      operator: lessThan
      value: 1s
  channels:
    - slack-alerts
    - pagerduty-critical
```

### HTTP Check with Authentication

```yaml
apiVersion: v1
kind: HttpCheck
metadata:
  name: authenticated-api-check
spec:
  url: https://api.example.com/protected
  method: POST
  headers:
    Authorization: Bearer ${API_TOKEN}
    Content-Type: application/json
  body: |
    {
      "query": "health"
    }
  interval: 5m
  checks:
    - type: statusCode
      value: 200
    - type: body
      operator: contains
      value: '"healthy":true'
```

### DNS Check with Multiple Record Types

```yaml
apiVersion: v1
kind: DnsCheck
metadata:
  name: mx-record-check
spec:
  hostname: example.com
  recordType: MX
  interval: 1h
  checks:
    - type: records
      operator: containsAny
      values:
        - "10 mail.example.com."
        - "20 mail2.example.com."
```

### Browser Check with Cookies

```yaml
apiVersion: browser/v1
kind: LoadCheck
metadata:
  name: authenticated-page-load
spec:
  url: https://app.example.com/dashboard
  interval: 10m
  browser:
    cookies:
      - name: session_id
        value: ${SESSION_TOKEN}
        domain: app.example.com
        secure: true
        httpOnly: true
  checks:
    - type: pageLoadTime
      operator: lessThan
      value: 3s
```

---

## Complete Example Repository

All examples are available in the model repository with validation tests:

📦 [syntheticopenschema/model/examples](https://github.com/syntheticopenschema/model/tree/main/examples)

These examples are:
- ✅ Tested against the reference implementation
- ✅ Validated on every commit
- ✅ Kept in sync with the specification
- ✅ Runnable with the Python runner

---

## Running Examples

### Using Python Runner

```bash
# Install the runner
pip install synthetic-open-schema-runner

# Run an example
sos-runner examples/http-check.yaml

# Run with verbose output
sos-runner --verbose examples/http-check.yaml
```

---

## Contributing Examples

Have a useful example? Contribute it!

1. Fork the [model repository](https://github.com/syntheticopenschema/model)
2. Add your example to `examples/`
3. Add a test to validate it
4. Submit a pull request

[Read contribution guidelines →](https://github.com/syntheticopenschema/spec/blob/main/CONTRIBUTING.md)
