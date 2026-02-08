# Implementations

Synthetic Open Schema can be implemented in any language. This page lists official and community implementations.

---

## Official Implementations

### Python

The reference implementation in Python, maintained by Ideatives Inc.

#### Model (Pydantic)

Pydantic-based models for all check types with strict validation.

```bash
pip install synthetic-open-schema-model
```

**Repository**: [syntheticopenschema/model](https://github.com/syntheticopenschema/model)
**License**: Apache 2.0
**Python**: 3.11+

**Features**:
- ✅ All v1 check types
- ✅ All browser/v1 check types
- ✅ Strict Pydantic validation
- ✅ JSON Schema generation
- ✅ Type hints throughout

**Usage**:

```python
from synthetic_open_schema_model.v1 import HttpCheck

# Parse YAML/JSON
check = HttpCheck.model_validate({
    "apiVersion": "v1",
    "kind": "HttpCheck",
    "metadata": {"name": "my-check"},
    "spec": {
        "url": "https://example.com",
        "interval": "1m",
        "checks": [{"type": "statusCode", "value": 200}]
    }
})

# Validate and use
print(check.spec.url)
```

---

#### Runner (Execution Engine)

Python execution engine for running checks.

```bash
pip install synthetic-open-schema-runner
```

**Repository**: [syntheticopenschema/runner](https://github.com/syntheticopenschema/runner)
**License**: Apache 2.0
**Python**: 3.11+

**Features**:
- ✅ Sync and async execution
- ✅ All v1 check types
- ✅ Browser checks (Playwright)
- ✅ Assertion evaluation
- ✅ Retry logic
- ✅ Result reporting

**CLI Usage**:

```bash
# Run a single check
sos-runner check.yaml

# Run with verbose output
sos-runner --verbose check.yaml

# Run with custom timeout
sos-runner --timeout 60 check.yaml
```

**Programmatic Usage**:

```python
from synthetic_open_schema_runner import run_check
from synthetic_open_schema_model.v1 import HttpCheck

# Create or load check
check = HttpCheck(...)

# Run synchronously
result = run_check(check)
print(f"Status: {result.success}")

# Run asynchronously
result = await run_check_async(check)
```

---

### JSON Schemas

JSON Schema definitions for all check types, generated from the Python model.

**Repository**: [syntheticopenschema/schemas](https://github.com/syntheticopenschema/schemas)
**License**: Apache 2.0

**Usage**:

```bash
# Download schemas
curl -O https://raw.githubusercontent.com/syntheticopenschema/schemas/main/v1.json
curl -O https://raw.githubusercontent.com/syntheticopenschema/schemas/main/browser-v1.json

# Validate with JSON Schema
jsonschema -i check.json v1.json
```

---

## Community Implementations

Built a conformant implementation? We'd love to list it here! [Open an issue](https://github.com/syntheticopenschema/spec/issues) with your implementation details.

---

## Resources

- 📖 [Read the Specification](specification/index.md)
- 💡 [View Examples](examples.md)
- 🐍 [Python Model Source](https://github.com/syntheticopenschema/model)
- 🏃 [Python Runner Source](https://github.com/syntheticopenschema/runner)
- 📊 [JSON Schemas](https://github.com/syntheticopenschema/schemas)

---

## Questions?

- 💬 [GitHub Discussions](https://github.com/syntheticopenschema/spec/discussions)
- 🐛 [Report Issues](https://github.com/syntheticopenschema/spec/issues)
- 📧 Contact through GitHub

**Ready to build?** We're excited to see what you create! 🚀
