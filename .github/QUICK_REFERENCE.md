# GitHub Actions Quick Reference

## 🚀 Quick Start

### For Contributors

1. **Fork the repository**
2. **Make your changes**
3. **Push to your fork** - Tests run automatically
4. **Create pull request** - Tests run again
5. **Wait for green checkmark** ✅

### For Maintainers

1. **Merge only when tests pass** ✅
2. **Check coverage reports**
3. **Review security warnings**

---

## 🔍 What Gets Tested?

### Every Push/PR Runs:

- ✅ **Syntax Check** - All Python files compile
- ✅ **Lint Check** - Flake8 code quality
- ✅ **Unit Tests** - Test suite with pytest
- ✅ **Coverage** - Code coverage reporting
- ✅ **Security** - Vulnerability scanning
- ✅ **Format Check** - Black formatting (optional)

---

## 📊 Viewing Test Results

### On GitHub

1. Go to **Actions** tab
2. Click on latest workflow run
3. View job results:
   - `test` - Full test suite
   - `syntax-check` - Quick syntax validation
   - `format-check` - Code formatting

### Build Status

```
✅ Green = All tests passed
❌ Red = Tests failed
🟡 Yellow = Tests running
```

---

## 🛠️ Running Tests Locally

### Quick Commands

```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=.

# Run specific test file
pytest test_erm.py

# Run specific test
pytest test_erm.py::ChecksTests::test_has_any_role_check_without_guild

# Verbose output
pytest -v

# Show print statements
pytest -s
```

### Code Quality

```bash
# Check syntax
python -m py_compile erm.py

# Lint code
flake8 .

# Format code
black .

# Security check
safety check
```

---

## 🔧 Common Issues & Fixes

### ❌ Tests Fail: Import Error

**Problem**: `ModuleNotFoundError`

**Fix**:
```bash
pip install -r requirements.txt
```

### ❌ Tests Fail: Environment Variables

**Problem**: Missing environment variables

**Fix**: Create `.env` file:
```bash
cp .env.template .env
# Edit .env with your values
```

### ❌ Flake8 Errors

**Problem**: Code doesn't meet style guide

**Fix**:
```bash
# See what's wrong
flake8 .

# Auto-format (fixes most issues)
black .

# Check again
flake8 .
```

### ❌ Tests Timeout

**Problem**: Tests take too long

**Fix**: Check for:
- Infinite loops
- Missing mocks for external services
- Network calls that should be mocked

---

## 📝 Environment Variables

### Required for Tests

```bash
ENVIRONMENT=DEVELOPMENT
MONGO_URL=mongodb://localhost:27017/test
DEVELOPMENT_BOT_TOKEN=test_token
BLOXLINK_API_KEY=test_key
```

### Optional

```bash
WEBGUI_HOST=127.0.0.1
WEBGUI_PORT=8080
AI_API_ENABLED=FALSE
```

---

## 🎯 Test Coverage

### View Coverage

```bash
# Generate HTML report
pytest --cov=. --cov-report=html

# Open report
open htmlcov/index.html  # macOS
xdg-open htmlcov/index.html  # Linux
```

### Coverage Goals

- 🎯 **Minimum**: 60%
- ✅ **Good**: 75%
- 🌟 **Excellent**: 90%+

---

## 🚨 Security Scanning

### Check for Vulnerabilities

```bash
# Install safety
pip install safety

# Scan dependencies
safety check

# Check specific file
safety check -r requirements.txt
```

---

## 📋 Checklist Before Pushing

- [ ] Tests pass locally: `pytest`
- [ ] Code is formatted: `black .`
- [ ] Linting passes: `flake8 .`
- [ ] Added tests for new features
- [ ] Updated documentation if needed
- [ ] Environment variables set correctly

---

## 💡 Pro Tips

### 1. Run Tests Before Pushing

```bash
# Quick pre-push check
pytest && flake8 . && echo "✅ Ready to push!"
```

### 2. Auto-format on Save

**VS Code** - Add to `.vscode/settings.json`:
```json
{
  "python.formatting.provider": "black",
  "editor.formatOnSave": true
}
```

**PyCharm** - Settings → Tools → Black

### 3. Skip CI for Docs

Add `[skip ci]` to commit message:
```bash
git commit -m "Update README [skip ci]"
```

### 4. Watch Test Coverage

```bash
# Watch mode - reruns on file changes
ptw --clear
```

Install with: `pip install pytest-watch`

---

## 🔄 Workflow Files

### Main Workflow
- **File**: `.github/workflows/app.yaml`
- **Triggers**: Push, Pull Request
- **Duration**: ~10-15 minutes

### Configuration Files
- **pytest.ini** - Test configuration
- **.flake8** - Linting rules
- **requirements.txt** - Dependencies

---

## 📚 Further Reading

- **Full Documentation**: `.github/GITHUB_ACTIONS.md`
- **Pytest Docs**: https://docs.pytest.org/
- **Flake8 Docs**: https://flake8.pycqa.org/
- **Black Docs**: https://black.readthedocs.io/

---

## 🆘 Getting Help

1. **Check the logs** - GitHub Actions tab
2. **Read error messages** - Usually very helpful
3. **Try locally** - Same commands as CI
4. **Ask for help** - Open an issue with:
   - Error message
   - Steps to reproduce
   - What you've tried

---

## 🎉 Success Indicators

### Green Checkmarks Mean:

✅ Code compiles without errors  
✅ All tests pass  
✅ Code meets style guidelines  
✅ No known security vulnerabilities  
✅ Ready to merge!

---

**Quick Help Commands**

```bash
# Test
pytest -v

# Lint
flake8 .

# Format
black .

# Coverage
pytest --cov=.

# Security
safety check

# All checks
pytest && flake8 . && black --check . && echo "✅ All good!"
```

---

**Version**: 1.0.0  
**Last Updated**: February 8, 2026  
**Status**: Production Ready ✅
