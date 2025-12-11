# 🚀 AWS Sandbox - Quick Start Guide

> **npm**: [aws-sandbox](https://www.npmjs.com/package/aws-sandbox) v0.3.2 | **AWS Solution**: [Innovation Sandbox on AWS](https://aws.amazon.com/solutions/implementations/innovation-sandbox-on-aws/)

---

## 💼 Business Value Proposition

| Challenge | Traditional Approach | With aws-sandbox | ROI |
|-----------|---------------------|------------------|-----|
| **AWS Testing Costs** | $200-500/month | **$0** (Tier 1+2) | 💰 100% savings |
| **Feedback Cycle** | 15-30 minutes | **< 5 seconds** | ⚡ 99% faster |
| **Test Coverage** | 40-60% | **90-100%** | 🎯 +50% defect detection |
| **Compliance Evidence** | Manual, days | **Automated, minutes** | 📋 Audit-ready |
| **AWS Bill Surprises** | High risk | **Zero risk** | 🛡️ No runaway costs |

---

## 🎯 What is Innovation Sandbox on AWS?

**Innovation Sandbox on AWS** is an official AWS Solution enabling organizations to create **secure, cost-controlled, recyclable sandbox accounts** for experimentation and learning.

### Key Capabilities

| Capability | Description | Business Impact |
|------------|-------------|-----------------|
| 💰 **Cost Control** | Automated spend limits, budget thresholds | Predictable cloud costs |
| 🏛️ **Governance** | Standardized SCPs across sandbox accounts | Consistent security posture |
| ♻️ **Lifecycle Management** | Automatic account recycling | Zero orphaned resources |
| 🖥️ **Self-Service** | Web UI for sandbox requests | Reduced IT overhead |
| 🔒 **Security** | Workload isolation, IAM Identity Center | Enterprise-grade protection |

### Use Cases

| Persona | Use Case | Benefit |
|---------|----------|---------|
| 🎓 **Educators** | Secure learning environments | Students experiment safely |
| 💡 **Innovators** | Safe experimentation | No production risk |
| 📚 **Trainers** | Hands-on AWS labs | Controlled cost training |
| 👩‍💻 **Developers** | Temporary environments | Automatic cleanup |

---

## 🚀 Quick Start (3 Options)

### Option 1: VSCode Devcontainer (Recommended) ⭐

**Zero local setup - everything runs in Docker**

🐳 **Image**: [nnthanh101/terraform](https://hub.docker.com/r/nnthanh101/terraform) (Chainguard Wolfi, 30+ DevSecOps tools)

```bash
# 1. Clone repository
git clone https://github.com/1xOps/xOps.git
cd xOps

# 2. Open in VSCode
code .
# → Click "Reopen in Container" when prompted

# 3. Run tests (inside container)
aws-sandbox --version        # v0.3.2
aws-sandbox test --tier=1    # 23 checks, 2-3s, $0
aws-sandbox test --tier=2    # 24 checks, 30s, $0
```

### Option 2: Taskfile Runner 🔧

**Task automation with evidence generation**

```bash
cd xOps

# Core commands
task quickstart              # Show this guide
task install                 # Install aws-sandbox from npm
task test                    # Run Tier 1+2 tests ($0)
task deploy                  # Deploy to LocalStack

# Evidence generation (Manager)
task aws-sandbox:evidence:generate    # Generate all evidence + HTML reports
```

### Option 3: npm Direct Install 📦

```bash
# Install globally (use --ignore-scripts for workspace compatibility)
npm install -g aws-sandbox --ignore-scripts

# Verify
aws-sandbox --version        # v0.3.2

# Run tests
aws-sandbox test --tier=1    # Tier 1: Connectivity
aws-sandbox test --tier=2    # Tier 2: LocalStack
```

---

## 📊 3-Tier Testing Strategy

**90% coverage at $0 cost** with progressive quality gates:

| Tier | Type | Checks | Duration | Cost | Coverage |
|------|------|--------|----------|------|----------|
| 🥇 **Tier 1** | Connectivity + Snapshot | 23 + 29 | 2-5 sec | **$0** | 70-80% |
| 🥈 **Tier 2** | LocalStack Integration | 24 + 11 | 30-60 sec | **$0** | +15-20% |
| 🥉 **Tier 3** | AWS Sandbox | 14 | 5-10 min | ~$50/mo | +5-10% |

### Execution Order (Manager/End-User)

| Step | Command | Description | Cost |
|------|---------|-------------|------|
| 1️⃣ | `task test:tier1` | Snapshot + connectivity | $0 (2-3 sec) |
| 2️⃣ | `task test:tier2` | LocalStack integration | $0 (30-60 sec) |
| 3️⃣ | `task deploy` | Deploy to LocalStack | $0 |
| 4️⃣ | `task aws-sandbox:evidence:generate` | Generate evidence | $0 |
| 5️⃣ | `task upgrade` | Upgrade to latest | $0 |

---

## 📋 Evidence Generation (Manager Portal)

Generate validation evidence for quality gates. **All commands reproducible** from xOps devcontainer.

### One-Command Generation

```bash
# Inside devcontainer: generates ALL evidence + HTML reports
task sandbox:evidence:generate
```

### Manual Step-by-Step

```bash
# 1. Ensure you're in the devcontainer
cd /workspace

# 2. Install aws-sandbox CLI
task install

# 3. Run tests
aws-sandbox test --tier=1    # Tier 1: 23 connectivity checks
aws-sandbox test --tier=2    # Tier 2: 24 LocalStack services

# 4. Generate HTML reports
task sandbox:evidence:html:report     # 06-evidence-report.html
task sandbox:evidence:html:frontend   # 07-frontend-tests.html
task sandbox:evidence:html:index      # index.html

# 5. Capture screenshots (optional)
task sandbox:screenshot:all
```

### Evidence Output Structure

```
/workspace/tmp/aws-sandbox/
├── 01-version.txt               # CLI version verification
├── 02-tier1-check.txt           # Tier 1 test results (23/23)
├── 03-tier2-check.txt           # Tier 2 test results (24/24)
├── 04-localstack-health.json    # LocalStack service status (22+ services)
├── 05-summary.md                # Business value summary
├── 06-evidence-report.html      # Infrastructure evidence (22+ services)
├── 07-frontend-tests.html       # Frontend test report (HTML)
├── index.html                   # 📊 Evidence Portal (HTML)
└── screenshots/
    ├── frontend-ui.svg          # Frontend test summary (SVG)
    └── localstack-infra.svg     # LocalStack services (SVG)
```

### View Reports

```bash
# Open evidence portal
open /workspace/tmp/aws-sandbox/index.html

# Direct links
open /workspace/tmp/aws-sandbox/06-evidence-report.html
```

---

## 🌐 LocalStack Service Coverage

### ✅ Fully Supported (Tier 1+2, $0)

| Category | Services | Status |
|----------|----------|--------|
| **Data** | S3, DynamoDB, DynamoDB Streams | ✅ Full CRUD |
| **Compute** | Lambda, API Gateway, Step Functions | ✅ Full execution |
| **IAM/Security** | IAM, STS, KMS, Secrets Manager | ✅ Policy enforcement |
| **Messaging** | SQS, SNS, EventBridge | ✅ Full pub/sub |
| **Config/Logging** | SSM, CloudWatch Logs, CloudFormation | ✅ Core operations |
| **Network/Certs** | EC2, Route53, ACM | ✅ VPC, DNS, TLS |
| **Streaming** | Kinesis | ✅ Stream processing |

### ⚠️ Tier 3 Required (AWS Only)

| Service | Stack | Reason |
|---------|-------|--------|
| AWS Organizations | AccountPool | Ultimate tier only |
| IAM Identity Center | IDC | Ultimate tier only |
| WAFv2 WebACL | Compute | Ultimate tier only |
| Service Control Policies | AccountPool | Org-level governance |

📖 **Reference**: [LocalStack Coverage Docs](https://docs.localstack.cloud/references/coverage/)

### CDK Stack Coverage

| Stack | Purpose | Tier 1+2 | Tier 3 |
|-------|---------|----------|--------|
| **IsbDataStack** | DynamoDB, KMS, AppConfig | ✅ $0 | ✅ |
| **IsbComputeStack** | Lambda, API Gateway, Step Functions | ✅ $0 | ✅ |
| **IsbAccountPoolStack** | Organizations, SCPs, StackSets | 📸 Snapshot | ✅ Required |
| **IsbIdcStack** | IAM Identity Center, SSO | 📸 Snapshot | ✅ Required |

**Key Insight**: DataStack + ComputeStack = **90% of code**, testable at **$0**

---

## 🛠️ Taskfile Commands Reference

### Core Commands

| Command | Description | Cost |
|---------|-------------|------|
| `task quickstart` | Show quick start guide | - |
| `task install` | Install aws-sandbox from npm | - |
| `task test` | Run Tier 1+2 tests | $0 |
| `task test:tier1` | Tier 1 only (snapshot) | $0 |
| `task test:tier2` | Tier 2 only (LocalStack) | $0 |
| `task deploy` | Deploy to LocalStack | $0 |
| `task upgrade` | Upgrade to latest version | - |

### Evidence Commands

| Command | Description | Output |
|---------|-------------|--------|
| `task sandbox:evidence:generate` | Generate all evidence | `tmp/aws-sandbox/` |
| `task sandbox:evidence:html:report` | Infrastructure report | `06-evidence-report.html` |
| `task sandbox:evidence:html:index` | Evidence portal | `index.html` |
| `task sandbox:screenshot:all` | Capture screenshots | `screenshots/` |

### Docker Commands

| Command | Description |
|---------|-------------|
| `task docker:start` | Start production containers |
| `task docker:stop` | Stop containers |
| `task docker:logs` | View LocalStack logs |
| `task docker:health` | Check LocalStack health |

---

## 🔒 Security & Compliance

### Security Features

| Feature | Implementation |
|---------|---------------|
| 🔐 **Encryption at Rest** | AES-256 (S3), AWS-managed KMS (DynamoDB) |
| 👤 **IAM** | Least-privilege, permission boundaries |
| 🔑 **Secrets** | Zero hardcoded credentials |
| 🔍 **Scanning** | Trivy + Checkov integrated |

### Compliance Coverage

| Framework | Requirement | Status |
|-----------|-------------|--------|
| **SOX Section 404** | 7-year retention | ✅ |
| **HIPAA** | 6-year retention, PITR | ✅ |
| **ADLC Constitution** | 35 checkpoints | ✅ |

---

## 📦 Package Information

| Property | Value |
|----------|-------|
| **npm** | [npmjs.com/package/aws-sandbox](https://www.npmjs.com/package/aws-sandbox) |
| **Version** | 0.3.3 |
| **License** | Apache-2.0 |
| **GitHub** | [github.com/1xOps/sandbox](https://github.com/1xOps/sandbox) |
| **Size** | ~1.3MB compressed / ~5.3MB unpacked |

### Installation

```bash
# Recommended (global with --ignore-scripts)
npm install -g aws-sandbox --ignore-scripts

# Verify installation
aws-sandbox --version
```

---

## 🔧 Troubleshooting

### npm install fails with "Workspaces not supported"

```bash
# Use --ignore-scripts flag
npm install -g aws-sandbox --ignore-scripts
```

### aws-sandbox command not found

```bash
# Reinstall
task install
# or
npm install -g aws-sandbox --ignore-scripts
```

### LocalStack not responding

```bash
# Check health
curl http://localhost:4566/_localstack/health | jq .

# Restart
task docker:stop && task docker:start
```

### task command not found

```bash
# Install task runner (macOS)
brew install go-task/tap/go-task

# Or download from https://taskfile.dev/
```

> `zip -r tmp.zip ./tmp/`

---

## 📚 Resources

| Resource | Link |
|----------|------|
| 📦 **npm Package** | [npmjs.com/package/aws-sandbox](https://www.npmjs.com/package/aws-sandbox) |
| 🐙 **GitHub** | [github.com/1xOps/sandbox](https://github.com/1xOps/sandbox) |
| ☁️ **AWS Solution** | [Innovation Sandbox on AWS](https://aws.amazon.com/solutions/implementations/innovation-sandbox-on-aws/) |
| 📖 **AWS Workshop** | [AWS Workshop](https://catalog.us-east-1.prod.workshops.aws/workshops/23f635fc-dc98-4f75-8b91-6f334d0e22c3/en-US) |
| 📝 **AWS Blog** | [Empowering Educators](https://aws.amazon.com/blogs/publicsector/empowering-educators-how-innovation-sandbox-on-aws-accelerates-learning-objectives-through-secure-cost-effective-and-recyclable-sandbox-management/) |
| 🏗️ **LocalStack** | [docs.localstack.cloud](https://docs.localstack.cloud/) |

---

## 📈 Cross-Validation Results (v0.3.2)

| Gate | Target | Result |
|------|--------|--------|
| Tier 1 Tests | 29/29 | ✅ **PASS** |
| Tier 2 Tests | 11/11 | ✅ **PASS** |
| Consumer Tier 1 | 23/23 | ✅ **PASS** |
| Consumer Tier 2 | 24/24 | ✅ **PASS** |
| Cross-Validation | ≥99.5% | ✅ **100% (47/47)** |
| Manager Agreement | ≥99.5% | ✅ **100%** |

---

**Version**: 0.3.2 | **ADLC**: v1.2.0 | **Updated**: 2025-12-11 | **Status**: ✅ Consumer CDK Deploy
