# AWS Sandbox - Quick Start Guide

> **npm package**: [aws-sandbox](https://www.npmjs.com/package/aws-sandbox) | **Based on**: [Innovation Sandbox on AWS](https://aws.amazon.com/solutions/implementations/innovation-sandbox-on-aws/)

## What is Innovation Sandbox on AWS?

**Innovation Sandbox on AWS** is an official AWS Solution that enables organizations to create secure, cost-controlled, recyclable sandbox accounts for experimentation and learning.

### Key Benefits

| Benefit | Description |
|---------|-------------|
| **Cost Control** | Automated spend limits, budget thresholds, account cleanup |
| **Governance** | Standardized SCPs across all sandbox accounts |
| **Lifecycle Management** | Automatic account recycling when limits reached |
| **Self-Service** | Web UI for users to request sandbox access |
| **Security** | Workload isolation, IAM Identity Center integration |

### Use Cases

- **Education**: Secure learning environments for students
- **Innovation**: Safe experimentation without production risk
- **Training**: Hands-on AWS labs with cost controls
- **Development**: Temporary environments with automatic cleanup

### Resources

- [AWS Solution Page](https://aws.amazon.com/solutions/implementations/innovation-sandbox-on-aws/)
- [AWS Workshop](https://catalog.us-east-1.prod.workshops.aws/workshops/23f635fc-dc98-4f75-8b91-6f334d0e22c3/en-US)
- [AWS Blog: Empowering Educators](https://aws.amazon.com/blogs/publicsector/empowering-educators-how-innovation-sandbox-on-aws-accelerates-learning-objectives-through-secure-cost-effective-and-recyclable-sandbox-management/)

---

## For External Users

### Option 1: VSCode/Codespaces (Recommended)

```bash
# 1. Clone repository
git clone https://github.com/1xOps/xOps.git
cd xOps

# 2. Open in VSCode
code .

# 3. When prompted, click "Reopen in Container"
#    aws-sandbox installs automatically via --ignore-scripts

# 4. Run tests
aws-sandbox test --tier=1
```

### Option 2: npm Install (Standalone)

```bash
# Use --ignore-scripts to avoid workspace issues
npm install -g aws-sandbox --ignore-scripts
aws-sandbox --version
aws-sandbox --help

# Install
npm install -g aws-sandbox@0.2.7 --ignore-scripts

# Test
aws-sandbox test --tier=1
aws-sandbox test --tier=2

# Deploy to LocalStack
aws-sandbox deploy --localstack
```

### Option 3: Task Runner

```bash
cd xOps
task quickstart          # Show quick start guide
task install             # Install aws-sandbox from npm
task test                # Run Tier 1+2 tests (free)
task test:tier1          # Tier 1 only (snapshot)
task test:tier2          # Tier 2 only (LocalStack)
task deploy              # Deploy to LocalStack
```

---

## Execution Order (Manager/End-User)

| Step | Command | Description | Cost |
|------|---------|-------------|------|
| 1 | `task test:tier1` | Snapshot tests | $0 (2-3 sec) |
| 2 | `task test:tier2` | LocalStack tests | $0 (30-60 sec) |
| 3 | `task deploy` | Deploy to LocalStack | $0 |
| 4 | `task sandbox:evidence:generate` | Evidence generation guide | $0 |
| 5 | `task upgrade` | Upgrade to latest version | $0 |

---

## Evidence Generation (Manager)

Generate validation evidence for quality gates. All commands are **reproducible** from the xOps devcontainer.

### Quick Start (One Command)

```bash
# Inside devcontainer: generates ALL evidence + HTML reports
task sandbox:evidence:generate
```

### Step-by-Step (Manual Reproduction)

```bash
# 1. Ensure you're in the devcontainer
cd /workspace

# 2. Install aws-sandbox CLI
task install

# 3. Run Tier 1 tests (snapshot)
aws-sandbox test --tier=1

# 4. Run Tier 2 tests (LocalStack)
aws-sandbox test --tier=2

# 5. Generate HTML reports
task sandbox:evidence:html:report    # 06-evidence-report.html
task sandbox:evidence:html:frontend  # 07-frontend-tests.html
task sandbox:evidence:html:index     # index.html

# 6. Capture screenshots (optional, requires Playwright)
task sandbox:screenshot:all
```

### Evidence Output Structure

```
/workspace/tmp/aws-sandbox/
├── 01-version.txt           # CLI version
├── 02-tier1-test.txt        # Tier 1 test results
├── 03-tier2-test.txt        # Tier 2 test results
├── 04-localstack-health.json # LocalStack status
├── 05-summary.md            # Markdown summary
├── 06-evidence-report.html  # Infrastructure evidence (HTML)
├── 07-frontend-tests.html   # Frontend test report (HTML)
├── index.html               # Evidence portal (HTML)
└── screenshots/
    ├── frontend-ui.png
    └── localstack-infra.png
```

### View Reports in Browser

```bash
# Open evidence portal
open /workspace/tmp/aws-sandbox/index.html

# Or individual reports
open /workspace/tmp/aws-sandbox/06-evidence-report.html
```

---

## LocalStack Deployment

Deploy to LocalStack for local testing (no AWS costs):

```bash
# From xOps directory (inside devcontainer)
task deploy                  # Deploy DataStack to LocalStack

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

## LocalStack Service Coverage

| Service | Tier 1 | Tier 2 | Tier 3 | Notes |
|---------|--------|--------|--------|-------|
| DynamoDB | ✅ Mock | ✅ Full | ✅ Full | Complete support |
| S3 | ✅ Mock | ✅ Full | ✅ Full | Complete support |
| Lambda | ✅ Mock | ✅ Full | ✅ Full | Runs actual code |
| API Gateway | ✅ Mock | ✅ Full | ✅ Full | REST API supported |
| CloudFormation | ✅ Synth | ✅ Full | ✅ Full | CDK-local uses cfn-local |
| IAM | ✅ Mock | ✅ Full | ✅ Full | Policies enforced |
| EventBridge | ✅ Mock | ✅ Full | ✅ Full | Rules and targets |
| Step Functions | ✅ Mock | ✅ Full | ✅ Full | State machines |
| SNS/SQS | ✅ Mock | ✅ Full | ✅ Full | Pub/sub |
| Organizations | ⚠️ Mock | ⚠️ Limited | ✅ Required | Account creation AWS-only |
| IAM Identity Center | ⚠️ Mock | ❌ N/A | ✅ Required | SSO requires real AWS |
| CloudFront | ⚠️ Mock | ⚠️ Pro | ✅ Full | LocalStack Pro feature |
| WAF | ⚠️ Mock | ❌ N/A | ✅ Required | No LocalStack support |

* **LocalStack Docs**: https://docs.localstack.cloud/user-guide/aws/feature-coverage/
* **Key Insight**:
    * [x] DataStack + ComputeStack = 90% of code, testable at $0
    * [ ] AccountPoolStack + IDCStack = Tier 3 required for runtime, but Tier 1 snapshots still validate templates

<details>
    <summary>LocalStack Service Coverage</summary>

    > Tier 3 Required (No Free LocalStack Support)

    | Service                  | Stack       | Reason               |
    |--------------------------|-------------|----------------------|
    | AWS Organizations        | AccountPool | Ultimate tier only   |
    | IAM Identity Center      | IDC         | Ultimate tier only   |
    | WAFv2                    | Compute     | Ultimate tier only   |
    | Service Control Policies | AccountPool | Org-level governance |

    > Tier 1+2 Safe (LocalStack Free)

    | Service                     | Confidence | Notes          |
    |-----------------------------|------------|----------------|
    | S3, DynamoDB                | High       | Full CRUD      |
    | Lambda, Step Functions      | High       | Full execution |
    | API Gateway REST            | High       | Routes, auth   |
    | SQS, SNS, EventBridge       | High       | Full support   |
    | KMS, IAM, CloudFormation    | High       | Core ops       |
    | Secrets Manager, CloudWatch | High       | Full support   |

</details>

---

## Package Info

- **npm**: https://www.npmjs.com/package/aws-sandbox
- **Version**: 0.2.7
- **License**: Apache-2.0
- **GitHub**: https://github.com/1xOps/sandbox
- **Size**: 1.3MB compressed / 5.3MB unpacked

```bash
# Install with --ignore-scripts for global install
npm install -g aws-sandbox --ignore-scripts
```

### CDK Stacks Included

| Stack | Purpose | Tier 1+2 | Tier 3 |
|-------|---------|----------|--------|
| **IsbDataStack** | DynamoDB tables, KMS, AppConfig | ✅ $0 | ✅ |
| **IsbComputeStack** | Lambda, API Gateway, Step Functions | ✅ $0 | ✅ |
| **IsbAccountPoolStack** | Organizations OUs, SCPs, StackSets | ⚠️ Snapshot | ✅ Required |
| **IsbIdcStack** | IAM Identity Center, SSO | ⚠️ Snapshot | ✅ Required |

### What's Included

```
aws-sandbox/
├── bin/                    # CLI entry points
├── source/
│   ├── infrastructure/     # CDK stacks (TypeScript)
│   ├── frontend/           # React 18 UI (pre-built)
│   ├── handlers/           # Lambda functions
│   └── commons/            # Shared types
└── cdk.json               # CDK configuration
```

---

## Troubleshooting

### npm install fails with "Workspaces not supported"

Use `--ignore-scripts` flag:
```bash
npm install -g aws-sandbox --ignore-scripts
```

### aws-sandbox command not found

Run install again:
```bash
task install
# or
npm install -g aws-sandbox --ignore-scripts
```

### task test:tier1 not found

Use full path:
```bash
task sandbox:test:tier1
# or from root
task test:tier1
```
