# aws-sandbox

## The separation is INTENTIONAL and CRITICAL:

| Component                   | Purpose                 | User Perspective               |
|-----------------------------|-------------------------|--------------------------------|
| Producer (sandbox/cdk)      | Source code development | Developer with TypeScript      |
| Consumer (xOps/aws-sandbox) | Real-world validation   | End user with npm package only |

Value of Separation:
1. xOps = Real Consumer - validates what ACTUAL users experience
2. Tests the PUBLISHED package, not local source code
3. Catches issues that only appear in npm install flow (path resolution, missing files)
4. Producer tests pass ≠ Consumer tests pass (proven by v0.3.3-v0.3.9)

## Local-First + CI/CD Integration > Workflow

LOCAL DEVELOPMENT (sandbox/cdk):
┌─────────────────────────────────────────────────────┐
│ 1. npm run build:consumer-app                       │
│ 2. npm publish --dry-run  (validates package)       │
│ 3. Review dry-run output (files, size, structure)   │
│ 4. npm publish (when ready)                         │
└─────────────────────────────────────────────────────┘
                        │
                        ▼
GITHUB ACTIONS (automatic trigger):
┌─────────────────────────────────────────────────────┐
│ 5. Detect new npm version (webhook or schedule)     │
│ 6. Clone xOps, install aws-sandbox@latest           │
│ 7. Run: task quickstart (47/47 consumer tests)      │
│ 8. IF FAIL → Create GitHub issue / Slack alert      │
└─────────────────────────────────────────────────────┘