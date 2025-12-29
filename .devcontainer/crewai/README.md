# CrewAI DevContainer Template - Enterprise Edition

**Purpose**: External DevContainer template for CrewAI projects using `nnthanh101/runbooks:ai`

**Version**: 1.0.0 - Enterprise Grade
**Date**: 2025-12-29

---

## 🎯 What Is This?

This is a **reusable DevContainer configuration** for CrewAI projects. Copy it to your project to get an enterprise-grade development environment instantly.

### Features ✅

- ✅ **Starship Prompt**: Beautiful, informative shell
- ✅ **AWS Integration**: Credentials mounted automatically
- ✅ **CrewAI 1.7.2**: Pre-installed and ready
- ✅ **VS Code Extensions**: Python, Ruff, Jupyter, AWS Toolkit, Task, Docker
- ✅ **Auto-Save**: Enabled (1 second delay)
- ✅ **Login Shell**: Full environment loaded
- ✅ **Port Forwarding**: Streamlit (8501), API (8000), Jupyter (8888)
- ✅ **Telemetry Opt-Out**: Privacy-first

---

## 📦 How to Use

### For NEW CrewAI Projects

1. **Copy this folder** to your project:
   ```bash
   cp -r /Volumes/Working/projects/xOps/.devcontainer/crewai \
         /path/to/your-project/.devcontainer
   ```

2. **Create `pyproject.toml`** in your project root:
   ```toml
   [project]
   name = "your-crewai-project"
   version = "0.1.0"
   requires-python = ">=3.13"
   dependencies = [
       "crewai[tools]>=1.7.2,<2.0.0",
       "openai>=1.83.0,<1.84",
       "anthropic>=0.71.0,<0.72",
       "pydantic>=2.11.7,<2.12",
   ]
   ```

3. **Open in VS Code**:
   ```bash
   cd /path/to/your-project
   code .
   ```

4. **Rebuild Container**:
   - `Cmd+Shift+P` → "Dev Containers: Reopen in Container"

5. **Start Developing**:
   - Dependencies auto-installed via UV
   - Starship prompt active
   - AWS credentials available
   - All extensions installed

---

### For EXISTING CrewAI Projects

1. **Copy `devcontainer.json`**:
   ```bash
   mkdir -p your-project/.devcontainer
   cp /Volumes/Working/projects/xOps/.devcontainer/crewai/devcontainer.json \
      your-project/.devcontainer/
   ```

2. **Adjust if needed**:
   ```json
   {
     "name": "Your Project Name",  // Customize
     "image": "docker.io/nnthanh101/runbooks:ai",
     // ... rest stays the same
   }
   ```

3. **Open in DevContainer**:
   - VS Code will detect `.devcontainer/devcontainer.json`
   - Prompt to "Reopen in Container"

---

## 🔧 Configuration Details

### What Happens on Container Creation

1. **onCreateCommand**: Validates environment
   ```bash
   ✅ Python: 3.13.11 | UV: 0.9.18 | CrewAI: 1.7.2
   ```

2. **postCreateCommand**: Installs dependencies (if `pyproject.toml` exists)
   ```bash
   uv sync --all-extras
   ✅ Dependencies installed
   ```
   OR (if no `pyproject.toml`):
   ```bash
   ⚠️  No pyproject.toml found - skipping uv sync (external DevContainer template)
   ```

3. **postStartCommand**: Ready message
   ```bash
   ✅ CrewAI DevContainer ready
   ```

4. **postAttachCommand**: Login shell
   ```bash
   bash --login  # Loads Starship, aliases, completions
   ```

---

### Mounted Directories

| Host Location | Container Location | Mode | Purpose |
|--------------|-------------------|------|---------|
| `~/.aws` | `/home/os/.aws` | read-only | AWS credentials |
| `~/.config/crewai` | `/home/os/.config/crewai` | read-write | CrewAI cache |

**Security**: AWS credentials are **read-only** to prevent accidental modification.

---

### Port Forwarding

| Port | Service | Auto-Forward |
|------|---------|--------------|
| 8501 | Streamlit UI | Notify |
| 8000 | FastAPI/MkDocs | Notify |
| 8888 | JupyterLab | Notify |

**Notifications**: Desktop notifications when ports auto-forward.

---

### VS Code Extensions (Auto-Installed)

1. **ms-python.python**: Python language support
2. **ms-python.vscode-pylance**: Fast type checking
3. **charliermarsh.ruff**: Linting & formatting
4. **ms-toolsai.jupyter**: Jupyter notebooks
5. **AmazonWebServices.aws-toolkit-vscode**: AWS development
6. **task.vscode-task**: Task runner integration
7. **ms-azuretools.vscode-docker**: Docker management

---

### Environment Variables

```bash
# Python
PYTHONPATH=/workspaces/your-project/src

# CrewAI
STORAGE_DIR=/workspaces/your-project/storage
OUTPUT_DIR=/workspaces/your-project/output
CREWAI_STORAGE_DIR=/workspaces/your-project/storage
CREWAI_TELEMETRY_OPT_OUT=true

# Terminal
TERM=xterm-256color
COLORTERM=truecolor
STARSHIP_CONFIG=/home/os/.config/starship.toml
```

---

## 🎨 Starship Prompt

You'll see a beautiful, informative prompt like:

```bash
🐍 3.13 🌩️ .VA ~/projects/your-project master ❯
```

Features:
- 🐍 Python version
- 🌩️ AWS region (if configured)
- Git branch
- Working directory
- Memory usage (if high)

---

## 🚀 Quick Start Example

### Minimal CrewAI Project

```bash
# 1. Create project
mkdir my-crew-project && cd my-crew-project

# 2. Copy DevContainer
cp -r /Volumes/Working/projects/xOps/.devcontainer/crewai .devcontainer

# 3. Create pyproject.toml
cat > pyproject.toml <<'EOF'
[project]
name = "my-crew-project"
version = "0.1.0"
requires-python = ">=3.13"
dependencies = [
    "crewai[tools]>=1.7.2,<2.0.0",
    "openai>=1.83.0,<1.84",
]
EOF

# 4. Create main.py
cat > main.py <<'EOF'
from crewai import Agent, Task, Crew

agent = Agent(
    role="Researcher",
    goal="Research AI topics",
    backstory="Expert AI researcher"
)

task = Task(
    description="Research CrewAI 1.7.2 features",
    agent=agent
)

crew = Crew(agents=[agent], tasks=[task])
result = crew.kickoff()
print(result)
EOF

# 5. Open in VS Code
code .

# 6. Reopen in Container (when prompted)
# 7. Run your crew
python main.py
```

---

## 🛠️ Query Installed Tools

```bash
# List all packages
pip list

# Search AI packages
pip list | grep -iE 'crew|openai|anthropic'

# Show package details
pip show crewai
```

---

## 🔍 Troubleshooting

### Issue: "`ll` command not found"

**Solution**: The base image doesn't include the `ll` alias by default. Use `ls -la` instead.

Or add it yourself once:
```bash
echo "alias ll='ls -la'" >> ~/.bashrc
source ~/.bashrc
```

---

### Issue: "No pyproject.toml found"

**Symptom**:
```
⚠️  No pyproject.toml found - skipping uv sync
```

**Solution**: This is **normal** for external DevContainer templates. Create a `pyproject.toml` in your project root:
```toml
[project]
name = "your-project"
dependencies = ["crewai[tools]>=1.7.2,<2.0.0"]
```

---

### Issue: Starship prompt not showing

**Solution**: Ensure you're using login shell:
```bash
bash --login
```
Or restart VS Code terminal.

---

### Issue: AWS credentials not available

**Solution**: Ensure `~/.aws/` exists on your host machine:
```bash
ls ~/.aws/
# Should show: config credentials
```

---

### Issue: Port already in use

**Solution**: Stop other services using the same port:
```bash
lsof -ti:8501 | xargs kill  # Stop Streamlit
lsof -ti:8888 | xargs kill  # Stop Jupyter
```

---

## 📝 Customization

### Change Port Numbers

Edit `.devcontainer/devcontainer.json`:
```json
"forwardPorts": [8502, 8001, 8889],  // Custom ports
"portsAttributes": {
  "8502": { "label": "My Streamlit" },
  // ...
}
```

### Add Custom Environment Variables

```json
"remoteEnv": {
  "PYTHONPATH": "${containerWorkspaceFolder}/src",
  "MY_CUSTOM_VAR": "value"  // Add here
}
```

### Add More VS Code Extensions

```json
"extensions": [
  "ms-python.python",
  "your-extension-id"  // Add here
]
```

---

## 🏆 Best Practices

### ✅ DO

- Use `pyproject.toml` for dependencies
- Keep `requirements.txt` minimal (or remove)
- Use UV for package management (`uv sync`)
- Commit `.devcontainer/` to git
- Use `CREWAI_TELEMETRY_OPT_OUT=true` for privacy

### ❌ DON'T

- Don't hardcode API keys in `devcontainer.json`
- Don't modify base image (`nnthanh101/runbooks:ai`)
- Don't remove login shell (`bash --login`)
- Don't disable Starship (it's awesome!)

---

## 📚 Related Documentation

- **Base Image**: [nnthanh101/runbooks:ai on Docker Hub](https://hub.docker.com/r/nnthanh101/runbooks/tags)
- **CrewAI Docs**: [https://docs.crewai.com](https://docs.crewai.com)
- **DevContainers**: [https://containers.dev](https://containers.dev)
- **Starship**: [https://starship.rs](https://starship.rs)

---

## 🆘 Support

### Internal (xOps Team)
- Check CloudOps-Runbooks `.devcontainer/` for reference
- See Resume AI project for working example

### External Users
- This is a template - customize for your needs
- Base image `nnthanh101/runbooks:ai` is public
- CrewAI community: [Discord](https://discord.gg/crewai)

---

## 📋 Changelog

### Version 1.0.0 (2025-12-29)
- ✅ Initial enterprise-grade template
- ✅ Starship integration
- ✅ AWS credentials mount
- ✅ CrewAI 1.7.2 support
- ✅ 7 essential VS Code extensions
- ✅ Login shell configuration
- ✅ Port attributes with labels
- ✅ Auto-save enabled
- ✅ Telemetry opt-out
- ✅ External project support (no pyproject.toml required)

---

**Created By**: Principal AI/DevOps Engineer
**License**: MIT (use freely)
**Status**: Production-Ready
