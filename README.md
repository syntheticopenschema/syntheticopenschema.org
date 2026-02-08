# Synthetic Open Schema Website

Official documentation website for [Synthetic Open Schema](https://syntheticopenschema.org), built with [MkDocs Material](https://squidfunk.github.io/mkdocs-material/).

[![Deploy Documentation](https://github.com/syntheticopenschema/syntheticopenschema.org/actions/workflows/deploy.yml/badge.svg)](https://github.com/syntheticopenschema/syntheticopenschema.org/actions/workflows/deploy.yml)
[![License](https://img.shields.io/badge/license-Apache%202.0-blue)](LICENSE)

---

## About

This repository contains the source for the Synthetic Open Schema documentation website at [syntheticopenschema.org](https://syntheticopenschema.org).

### Key Features

- **Auto-Sync Specification**: Pulls spec docs from [syntheticopenschema/spec](https://github.com/syntheticopenschema/spec) at build time
- **MkDocs Material**: Beautiful, searchable documentation with dark mode
- **GitHub Pages**: Automatically deployed on push to main
- **Daily Rebuilds**: Keeps docs fresh with latest spec changes

---

## Structure

```
syntheticopenschema.org/
├── mkdocs.yml              # MkDocs configuration
├── pyproject.toml          # Python project configuration (uv)
├── .github/
│   └── workflows/
│       └── deploy.yml      # Auto-deploy workflow
└── docs/
    ├── index.md            # Homepage (custom)
    ├── getting-started.md  # Quick start guide (custom)
    ├── specification/      # Auto-populated from spec repo
    │   ├── index.md        # Spec overview (custom)
    │   ├── v1/             # ← Copied at build time
    │   ├── browser-v1/     # ← Copied at build time
    │   └── policy/         # ← Copied at build time
    ├── examples.md         # Examples (custom)
    └── implementations.md  # Implementations (custom)
```

---

## Local Development

### Prerequisites

- Python 3.11+
- [uv](https://github.com/astral-sh/uv) (recommended) or pip

### Setup

```bash
# Clone the repository
git clone https://github.com/syntheticopenschema/syntheticopenschema.org.git
cd syntheticopenschema.org

# Install dependencies with uv (recommended)
uv sync

# Or with pip
pip install -e .
```

### Preview Locally

```bash
# Start development server
mkdocs serve

# Visit http://localhost:8000
```

The dev server watches for changes and reloads automatically.

### Build Static Site

```bash
# Build site to site/ directory
mkdocs build

# Build with verbose output
mkdocs build --verbose
```

---

## Auto-Sync Specification Docs

Specification documents are **not committed** to this repository. They are automatically fetched from the [spec repository](https://github.com/syntheticopenschema/spec) at build time.

### How It Works

1. **On Push**: When you push to `main`, GitHub Actions triggers
2. **Clone Spec Repo**: Workflow clones `syntheticopenschema/spec`
3. **Copy Docs**: Copies markdown files to `docs/specification/`
4. **Build Site**: Runs `mkdocs build`
5. **Deploy**: Pushes to `gh-pages` branch

### Manual Sync (Local Development)

To test with the latest spec docs locally:

```bash
# Clone spec repo (temporary)
git clone https://github.com/syntheticopenschema/spec.git spec-repo

# Copy docs
mkdir -p docs/specification/v1
mkdir -p docs/specification/browser-v1
mkdir -p docs/specification/policy

cp spec-repo/v1/*.md docs/specification/v1/
cp spec-repo/browser/v1/*.md docs/specification/browser-v1/
cp spec-repo/glossary.md docs/specification/policy/
cp spec-repo/versioning.md docs/specification/policy/
cp spec-repo/compatibility.md docs/specification/policy/

# Preview
mkdocs serve

# Clean up
rm -rf spec-repo
```

---

## Deployment

### Automatic (Recommended)

Deployment happens automatically via GitHub Actions:

- **On Push to Main**: Rebuilds and deploys immediately
- **Daily at 2am UTC**: Rebuilds to catch spec updates
- **Manual Trigger**: Via GitHub Actions UI

### Manual Deploy

```bash
# Deploy to GitHub Pages
mkdocs gh-deploy

# Deploy with custom message
mkdocs gh-deploy --message "Custom deploy message"
```

---

## GitHub Pages Configuration

### Custom Domain

1. Go to repository Settings → Pages
2. Set custom domain: `syntheticopenschema.org`
3. Enable "Enforce HTTPS"
4. Wait for DNS verification

### DNS Configuration

Point your domain to GitHub Pages:

```
# A Records
Type: A
Name: @
Value: 185.199.108.153
       185.199.109.153
       185.199.110.153
       185.199.111.153

# CNAME Record
Type: CNAME
Name: www
Value: syntheticopenschema.github.io
```

---

## Contributing

### Content Updates

Custom pages (homepage, getting started, examples) are in `docs/`:

```
docs/
├── index.md            # Edit homepage
├── getting-started.md  # Edit quick start
├── specification/
│   └── index.md        # Edit spec overview
├── examples.md         # Edit examples page
└── implementations.md  # Edit implementations page
```

### Specification Updates

**Don't edit spec docs here!** They're auto-synced from the spec repository.

To update specification documents:
1. Submit changes to [syntheticopenschema/spec](https://github.com/syntheticopenschema/spec)
2. Website rebuilds automatically (daily or on push)

### Style/Theme Updates

- **Configuration**: Edit `mkdocs.yml`
- **Custom Styles**: Add to `docs/stylesheets/extra.css`
- **Colors**: Update `theme.palette` in `mkdocs.yml`

### Workflow

1. Fork this repository
2. Create a feature branch
3. Make your changes
4. Test locally with `mkdocs serve`
5. Submit a pull request

---

## Troubleshooting

### Build Fails on GitHub Actions

Check the [Actions tab](https://github.com/syntheticopenschema/syntheticopenschema.org/actions) for logs.

Common issues:
- Missing files in spec repo
- Invalid markdown syntax
- Broken links

### Local Build Issues

```bash
# Clear MkDocs cache
rm -rf site/

# Reinstall dependencies
uv sync --reinstall

# Rebuild
mkdocs build --verbose
```

### Spec Docs Not Updating

The website rebuilds:
- On every push to main
- Daily at 2am UTC (cron schedule)
- Manually via Actions UI

To force an update:
1. Go to Actions tab
2. Select "Deploy Documentation" workflow
3. Click "Run workflow"

---

## Technology Stack

- **[MkDocs](https://www.mkdocs.org/)** — Static site generator
- **[Material for MkDocs](https://squidfunk.github.io/mkdocs-material/)** — Theme
- **[GitHub Pages](https://pages.github.com/)** — Hosting
- **[GitHub Actions](https://github.com/features/actions)** — CI/CD

---

## License

This website content is licensed under the **Apache License 2.0**.

The Synthetic Open Schema specification is also licensed under Apache 2.0.

---

## Links

- 🌐 **Website**: [syntheticopenschema.org](https://syntheticopenschema.org)
- 📖 **Specification**: [github.com/syntheticopenschema/spec](https://github.com/syntheticopenschema/spec)
- 🐍 **Python Model**: [github.com/syntheticopenschema/model](https://github.com/syntheticopenschema/model)
- 🏃 **Python Runner**: [github.com/syntheticopenschema/runner](https://github.com/syntheticopenschema/runner)

---

**Maintained by** [Ideatives Inc.](https://ideatives.com)
