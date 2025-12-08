# AWS Sandbox - Quick Start Guide

## For External Users

### Option 1: VSCode/Codespaces (Recommended)

```bash
# 1. Clone repository
git clone https://github.com/1xOps/xOps.git
cd xOps

# 2. Open in VSCode
code .

# 3. When prompted, click "Reopen in Container"
#    aws-sandbox installs automatically

# 4. Run tests
aws-sandbox test --tier=1
```

### Option 2: npm Install (Standalone)

```bash
npm install -g aws-sandbox
aws-sandbox --version
aws-sandbox --help
```

### Option 3: Task Runner

```bash
cd xOps
task quickstart          # Show quick start guide
task install             # Install aws-sandbox from npm
task test                # Run Tier 1+2 tests (free)
task deploy              # Deploy to LocalStack
```

---

## Execution Order (Manager/End-User)

| Step | Command | Description |
|------|---------|-------------|
| 1 | `task test:tier1` | Snapshot tests (2-3 sec, $0) |
| 2 | `task test:tier2` | LocalStack tests (30-60 sec, $0) |
| 3 | `task deploy` | Deploy to LocalStack |
| 4 | `task upgrade` | Upgrade to latest version |

---

## Package Info

- **npm**: https://www.npmjs.com/package/aws-sandbox
- **Version**: 0.2.1
- **License**: Apache-2.0

```bash
npm install -g aws-sandbox@0.2.1
```
