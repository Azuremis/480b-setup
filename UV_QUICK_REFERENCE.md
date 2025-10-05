# UV Quick Reference Card

Fast reference guide for UV package manager commands in the Qwen3-Coder-480B setup.

## 🚀 Installation & Setup

```bash
# Install UV
curl -LsSf https://astral.sh/uv/install.sh | sh

# Add UV to PATH (current session)
source "$HOME/.cargo/env"

# Add UV to PATH (permanent)
echo 'source "$HOME/.cargo/env"' >> ~/.bashrc

# Check UV version
uv --version

# Update UV
curl -LsSf https://astral.sh/uv/install.sh | sh
```

## 🐍 Virtual Environment

```bash
# Create virtual environment
uv venv

# Create with specific Python version
uv venv --python 3.10

# Create in custom location
uv venv /path/to/venv

# Activate environment
source .venv/bin/activate

# Deactivate environment
deactivate
```

## 📦 Package Installation

```bash
# Install single package
uv pip install <package>

# Install specific version
uv pip install <package>==1.0.0

# Install with version constraint
uv pip install "package>=1.0,<2.0"

# Install from requirements file
uv pip install -r requirements.txt

# Install from git
uv pip install git+https://github.com/user/repo.git

# Install from local directory
uv pip install -e /path/to/package

# Install with specific index
uv pip install torch --index-url https://download.pytorch.org/whl/cu121

# Install multiple packages
uv pip install package1 package2 package3
```

## 📋 Package Information

```bash
# List installed packages
uv pip list

# List in requirements format
uv pip freeze

# Show package details
uv pip show <package>

# Show dependency tree
uv pip tree

# Show specific package tree
uv pip tree <package>

# Check for outdated packages
uv pip list --outdated
```

## 🔄 Package Updates

```bash
# Update single package
uv pip install --upgrade <package>

# Update all packages (be careful!)
uv pip list --outdated | cut -d' ' -f1 | xargs -n1 uv pip install -U

# Reinstall package
uv pip install --force-reinstall <package>

# Reinstall with dependencies
uv pip install --force-reinstall --no-deps <package>
```

## 🗑️ Package Removal

```bash
# Uninstall single package
uv pip uninstall <package>

# Uninstall multiple packages
uv pip uninstall package1 package2

# Uninstall from requirements file
uv pip uninstall -r requirements.txt

# Uninstall with confirmation
uv pip uninstall <package> -y
```

## 💾 Cache Management

```bash
# Show cache directory
uv cache dir

# Show cache size
du -sh $(uv cache dir)

# Clean entire cache
uv cache clean

# Prune old cache entries
uv cache prune

# Show what would be pruned (dry-run)
uv cache prune --dry-run
```

## 🔍 Search & Inspect

```bash
# Search for packages (uses PyPI)
# Note: UV doesn't have built-in search, use pip search or PyPI web

# Inspect package without installing
uv pip show <package> --no-install

# Check if package exists
uv pip show <package> --quiet
```

## 📊 Environment Management

```bash
# Export environment
uv pip freeze > requirements.txt

# Sync environment from file
uv pip sync requirements.txt

# Install from pyproject.toml
uv pip install -e .

# Install with optional dependencies
uv pip install -e ".[dev]"
uv pip install -e ".[test,docs]"
```

## 🛠️ Troubleshooting Commands

```bash
# Verify environment
python -c "import sys; print(sys.executable)"

# Check installed version
python -c "import package; print(package.__version__)"

# List all files for a package
uv pip show -f <package>

# Check dependency conflicts
uv pip check

# Reinstall everything
uv pip freeze | uv pip uninstall -r /dev/stdin
uv pip install -r requirements.txt

# Clear cache and reinstall
uv cache clean
uv pip install --force-reinstall <package>
```

## ⚡ Qwen3-Coder-480B Specific

```bash
# Activate Qwen environment
source ~/activate_qwen480b.sh

# Install PyTorch with CUDA
uv pip install torch torchvision torchaudio \
  --index-url https://download.pytorch.org/whl/cu121

# Check PyTorch installation
python -c "import torch; print(torch.__version__, torch.cuda.is_available())"

# View ML dependencies
uv pip tree torch

# Update transformers
uv pip install --upgrade transformers

# Check GPU memory (after activation)
python -c "import torch; print(f'{torch.cuda.memory_allocated()/1e9:.1f}GB')"
```

## 📈 Performance Tips

```bash
# Use parallel downloads (automatic in UV)
uv pip install package1 package2 package3

# Leverage cache for faster reinstalls
# (automatic - UV caches by default)

# Install from local wheels
uv pip install --find-links ./wheels <package>

# Skip dependency checks (faster but risky)
uv pip install --no-deps <package>

# Use specific Python for speed
uv venv --python python3.10
```

## 🔒 Security & Verification

```bash
# Verify package checksums (automatic in UV)
# UV verifies all packages automatically

# Install from trusted sources only
uv pip install --index-url https://pypi.org/simple <package>

# Audit dependencies
uv pip tree | grep -i "security"

# Check for vulnerable packages
# Use external tools like pip-audit:
pip-audit  # Note: pip-audit works with UV environments
```

## 📝 Common Workflows

### Fresh Install
```bash
uv venv --python 3.10
source .venv/bin/activate
uv pip install -r requirements.txt
```

### Update Environment
```bash
source .venv/bin/activate
uv pip list --outdated
uv pip install --upgrade <package>
uv pip freeze > requirements.txt
```

### Migrate from pip
```bash
# Save current environment
pip freeze > old_requirements.txt

# Install UV
curl -LsSf https://astral.sh/uv/install.sh | sh
source "$HOME/.cargo/env"

# Create new UV environment
uv venv --python 3.10
source .venv/bin/activate

# Install packages with UV
uv pip install -r old_requirements.txt
```

### Debug Installation
```bash
# Check environment
which python
python --version
uv --version

# Check package
uv pip show <package>
uv pip tree <package>

# Reinstall clean
uv cache clean
uv pip uninstall <package>
uv pip install <package>
```

## 🎯 Best Practices

1. **Always use virtual environments**
   ```bash
   uv venv --python 3.10
   source .venv/bin/activate
   ```

2. **Keep UV updated**
   ```bash
   curl -LsSf https://astral.sh/uv/install.sh | sh
   ```

3. **Use pyproject.toml for projects**
   ```toml
   [project]
   dependencies = [
       "torch==2.3.0",
       "transformers>=4.50.0",
   ]
   ```

4. **Pin versions in production**
   ```bash
   uv pip freeze > requirements.txt
   ```

5. **Clean cache periodically**
   ```bash
   uv cache clean  # Run monthly
   ```

## 🆘 Quick Troubleshooting

| Problem | Solution |
|---------|----------|
| `uv: command not found` | `source "$HOME/.cargo/env"` |
| Wrong Python version | `uv venv --python 3.10` |
| Package conflicts | `uv cache clean && uv pip install <package>` |
| Slow installation | Check network, use `--index-url` |
| CUDA version wrong | Use correct PyTorch index URL |
| Cache too large | `uv cache clean` |
| Import error | `python -c "import sys; print(sys.path)"` |

## 📚 Additional Help

- **UV Help**: `uv --help`
- **Command Help**: `uv pip install --help`
- **Full Documentation**: https://github.com/astral-sh/uv
- **Qwen Setup Guide**: [README.md](./README.md)
- **Migration Guide**: [UV_MIGRATION_GUIDE.md](./UV_MIGRATION_GUIDE.md)

---

**Print this guide** and keep it handy for quick reference!  
**Bookmark this page**: Essential for daily UV usage.

