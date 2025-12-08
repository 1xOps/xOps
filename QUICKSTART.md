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
npm install -g aws-sandbox@0.2.1
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

| Step | Command | Description | Cost |
|------|---------|-------------|------|
| 1 | `task test:tier1` | Snapshot tests | $0 (2-3 sec) |
| 2 | `task test:tier2` | LocalStack tests | $0 (30-60 sec) |
| 3 | `task deploy` | Deploy to LocalStack | $0 |
| 4 | `task evidence:generate` | Generate evidence-report.html | $0 |
| 5 | `task upgrade` | Upgrade to latest version | $0 |

---

## Evidence Generation (Manager)

Generate validation evidence for quality gates:

```bash
# From sandbox/cdk directory (internal dev)
cd /Volumes/Working/projects/sandbox/cdk

# Generate evidence report
task docs:screenshot:html    # Generate evidence-report.html

# View report
task docs:screenshot:view    # Open in browser

# Full evidence capture
task evidence:capture        # CLI outputs + HTML report
```

**Evidence Output**:
- `tmp/cdk/evidence-report.html` - Main validation report
- `tmp/cdk/test-results/` - Test execution logs
- `tmp/cdk/screenshots/` - Visual evidence

---

## LocalStack Deployment

Deploy to LocalStack for local testing (no AWS costs):

```bash
# From sandbox/cdk directory
task deploy:localstack       # Deploy DataStack to LocalStack

# Verify deployment
aws --endpoint-url=http://localhost:4566 dynamodb list-tables
aws --endpoint-url=http://localhost:4566 s3 ls
```

---

## Tier Comparison

| Tier | Test Type | Cost | Duration | Coverage |
|------|-----------|------|----------|----------|
| 1 | Snapshot | $0 | 2-3 sec | 70-80% |
| 2 | LocalStack | $0 | 30-60 sec | +15-20% |
| 3 | AWS Sandbox | ~$50/mo | 5-10 min | +5-10% |

**90% of testing at $0 cost** with Tier 1+2 (LocalStack)

---

## Package Info

- **npm**: https://www.npmjs.com/package/aws-sandbox
- **Version**: 0.2.1
- **License**: Apache-2.0
- **GitHub**: https://github.com/1xOps/sandbox

```bash
npm install -g aws-sandbox@0.2.1
```
