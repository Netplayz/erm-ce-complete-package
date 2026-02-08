# GitHub Actions CI/CD Documentation

## Overview

This project uses GitHub Actions for Continuous Integration (CI) to automatically test code changes and ensure quality standards are maintained.

## Workflows

### Main Workflow: `app.yaml`

Located at `.github/workflows/app.yaml`, this workflow runs on every push and pull request.

#### Jobs

**1. Test Job** (`test`)
- **Purpose**: Run the full test suite with code coverage
- **Duration**: ~10-15 minutes
- **Runs on**: Ubuntu Latest

**Steps:**
1. ✅ Checkout code
2. ✅ Setup Python 3.12
3. ✅ Install system dependencies (git)
4. ✅ Upgrade pip
5. ✅ Install test dependencies (pytest, flake8)
6. ✅ Install project dependencies
7. ✅ Lint with Flake8
8. ✅ Check for debug statements
9. ✅ Run tests with coverage
10. ✅ Upload coverage reports
11. ✅ Security check with Safety
12. ✅ Generate summary

**2. Syntax Check Job** (`syntax-check`)
- **Purpose**: Quick validation of Python syntax
- **Duration**: ~2-3 minutes
- **Runs on**: Ubuntu Latest

**Steps:**
1. ✅ Checkout code
2. ✅ Setup Python 3.12
3. ✅ Compile all Python files

**3. Format Check Job** (`format-check`)
- **Purpose**: Check code formatting with Black
- **Duration**: ~2-3 minutes
- **Runs on**: Ubuntu Latest
- **Note**: Non-blocking (continues on error)

**Steps:**
1. ✅ Checkout code
2. ✅ Setup Python 3.12
3. ✅ Install Black
4. ✅ Check formatting

## Environment Variables

The workflow sets all required environment variables for testing:

### Required Variables
```yaml
ENVIRONMENT: "DEVELOPMENT"
MONGO_URL: "mongodb://localhost:27017/test"
```

### Bot Tokens (Placeholders)
```yaml
PRODUCTION_BOT_TOKEN: "test_token_placeholder_prod"
DEVELOPMENT_BOT_TOKEN: "test_token_placeholder_dev"
ALPHA_BOT_TOKEN: "test_token_placeholder_alpha"
```

### API Keys (Placeholders)
```yaml
BLOXLINK_API_KEY: "test_bloxlink_key"
PRC_API_KEY: "test_prc_key"
PRC_API_URL: "https://api.example.com"
```

### WebGUI Settings
```yaml
WEBGUI_HOST: "127.0.0.1"
WEBGUI_PORT: "8080"
```

## Running Tests Locally

### Quick Start

```bash
# Install dependencies
pip install -r requirements.txt

# Run tests
pytest

# Run with coverage
pytest --cov=. --cov-report=html

# Open coverage report
open htmlcov/index.html  # macOS
xdg-open htmlcov/index.html  # Linux
start htmlcov/index.html  # Windows
```

### With Environment Variables

Create a `.env` file (use `.env.template` as a guide) or export variables:

```bash
export ENVIRONMENT="DEVELOPMENT"
export MONGO_URL="mongodb://localhost:27017/test"
export DEVELOPMENT_BOT_TOKEN="your_test_token"
export BLOXLINK_API_KEY="your_test_key"
# ... other variables

pytest
```

### Run Specific Tests

```bash
# Run a specific test file
pytest test_erm.py

# Run a specific test class
pytest test_erm.py::ChecksTests

# Run a specific test method
pytest test_erm.py::ChecksTests::test_has_any_role_check_without_guild

# Run with verbose output
pytest -v

# Run with print statements shown
pytest -s

# Run tests matching pattern
pytest -k "role"
```

## Code Quality Checks

### Flake8 (Linting)

```bash
# Check for critical errors only
flake8 . --select=E9,F63,F7,F82

# Full check with warnings
flake8 .

# Ignore specific errors
flake8 . --ignore=E501,W503

# Check specific files
flake8 erm.py cogs/
```

### Black (Formatting)

```bash
# Check formatting
black --check .

# See what would change
black --diff .

# Auto-format all files
black .

# Format specific files
black erm.py cogs/
```

### Safety (Security)

```bash
# Install safety
pip install safety

# Check for vulnerabilities
safety check

# Check specific requirements file
safety check -r requirements.txt

# Output as JSON
safety check --json
```

## Pytest Configuration

Configuration is in `pytest.ini`:

```ini
[pytest]
# Test discovery
python_files = test_*.py *_test.py
python_classes = Test* *Tests
python_functions = test_*

# Minimum Python version
minversion = 3.10

# Show extra info
addopts = -v --strict-markers --tb=short

# Asyncio mode
asyncio_mode = auto
```

## Coverage Reports

### Terminal Output

```bash
pytest --cov=. --cov-report=term-missing
```

Shows coverage with line numbers that are missing coverage.

### HTML Report

```bash
pytest --cov=. --cov-report=html
```

Generates interactive HTML report in `htmlcov/`.

### XML Report (for CI)

```bash
pytest --cov=. --cov-report=xml
```

Generates `coverage.xml` for tools like Codecov.

## Continuous Integration

### When Tests Run

- ✅ Every push to `main` or `develop` branches
- ✅ Every pull request to `main` or `develop` branches
- ✅ Manual trigger from GitHub Actions tab

### Build Status

Check build status at:
```
https://github.com/YOUR_USERNAME/YOUR_REPO/actions
```

### Build Badge

Add to README.md:
```markdown
![Tests](https://github.com/YOUR_USERNAME/YOUR_REPO/workflows/PyTest%20and%20Code%20Quality/badge.svg)
```

## Troubleshooting

### Test Failures

**Issue**: Tests fail in CI but pass locally

**Solutions**:
1. Check Python version matches (3.12)
2. Ensure all environment variables are set
3. Check for OS-specific code (Linux in CI)
4. Look for file path issues (case sensitivity)
5. Check for timezone dependencies

**Example Debug:**
```bash
# Run tests with same Python version
python3.12 -m pytest

# Run with same environment
ENVIRONMENT=DEVELOPMENT pytest

# Check for skipped tests
pytest -v -rs
```

### Dependency Installation Failures

**Issue**: `pip install -r requirements.txt` fails

**Solutions**:
1. Check for conflicting dependencies
2. Ensure git is installed (for discord.py)
3. Clear pip cache: `pip cache purge`
4. Try installing individually to find problem

**Example Debug:**
```bash
# Update pip first
pip install --upgrade pip

# Try installing without cache
pip install -r requirements.txt --no-cache-dir

# Install with verbose output
pip install -r requirements.txt -v
```

### Import Errors

**Issue**: `ModuleNotFoundError` in tests

**Solutions**:
1. Check all dependencies installed
2. Verify Python path includes project root
3. Check for circular imports
4. Ensure `__init__.py` files exist

**Example Fix:**
```bash
# Install in development mode
pip install -e .

# Add project to Python path
export PYTHONPATH="${PYTHONPATH}:$(pwd)"
```

### Async Test Issues

**Issue**: Async tests not running or hanging

**Solutions**:
1. Install pytest-asyncio: `pip install pytest-asyncio`
2. Check pytest.ini has `asyncio_mode = auto`
3. Ensure test functions are marked with `async def`
4. Check for proper `await` usage

**Example Fix:**
```python
# In pytest.ini
[pytest]
asyncio_mode = auto

# In test file
async def test_async_function():
    result = await some_async_function()
    assert result is not None
```

## Best Practices

### Writing Tests

1. **Use descriptive names**
   ```python
   def test_has_any_role_check_with_guild_and_required_role():
       # Clear what this tests
   ```

2. **One assertion per test** (when possible)
   ```python
   def test_cpu_percent_is_float():
       cpu = get_cpu_percent()
       assert isinstance(cpu, float)
   
   def test_cpu_percent_in_valid_range():
       cpu = get_cpu_percent()
       assert 0 <= cpu <= 100
   ```

3. **Use fixtures for setup**
   ```python
   @pytest.fixture
   def mock_bot():
       return MockBot()
   
   def test_something(mock_bot):
       # Use mock_bot
   ```

4. **Test edge cases**
   ```python
   def test_with_empty_list():
       assert process([]) == []
   
   def test_with_none():
       assert process(None) == None
   ```

### Code Quality

1. **Keep lines under 127 characters**
2. **Use type hints**
   ```python
   def calculate(x: int, y: int) -> int:
       return x + y
   ```

3. **Add docstrings**
   ```python
   def complex_function(data: dict) -> list:
       """
       Process data and return results.
       
       Args:
           data: Dictionary containing input data
           
       Returns:
           List of processed items
       """
   ```

4. **Avoid bare except**
   ```python
   # Bad
   try:
       risky_operation()
   except:
       pass
   
   # Good
   try:
       risky_operation()
   except (ValueError, TypeError) as e:
       logging.error(f"Operation failed: {e}")
   ```

## GitHub Secrets (Optional)

For sensitive data in CI, use GitHub Secrets:

1. Go to repo → Settings → Secrets and variables → Actions
2. Add secrets like:
   - `PRODUCTION_BOT_TOKEN`
   - `REAL_API_KEYS` (if needed for integration tests)

3. Use in workflow:
   ```yaml
   - name: Run integration tests
     env:
       BOT_TOKEN: ${{ secrets.PRODUCTION_BOT_TOKEN }}
     run: pytest integration_tests/
   ```

## Notifications

### Slack Integration

Add to workflow:
```yaml
- name: Slack Notification
  uses: 8398a7/action-slack@v3
  if: always()
  with:
    status: ${{ job.status }}
    webhook_url: ${{ secrets.SLACK_WEBHOOK }}
```

### Email Notifications

GitHub sends emails automatically for:
- Failed workflows on your branches
- All workflows on pull requests you created

Configure in: GitHub → Settings → Notifications

## Maintenance

### Updating Dependencies

```bash
# Check for outdated packages
pip list --outdated

# Update specific package
pip install --upgrade package_name

# Update all packages (careful!)
pip install --upgrade -r requirements.txt

# Freeze to requirements
pip freeze > requirements.txt
```

### Updating Python Version

1. Update in workflow:
   ```yaml
   - name: Setup Python
     uses: actions/setup-python@v4
     with:
       python-version: "3.13"  # New version
   ```

2. Update in pytest.ini:
   ```ini
   [pytest]
   minversion = 3.13
   ```

3. Test locally with new version
4. Update documentation

## Additional Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Pytest Documentation](https://docs.pytest.org/)
- [Flake8 Documentation](https://flake8.pycqa.org/)
- [Black Documentation](https://black.readthedocs.io/)
- [Coverage.py Documentation](https://coverage.readthedocs.io/)

## Support

If you encounter issues with CI/CD:

1. Check the workflow logs on GitHub
2. Review this documentation
3. Try reproducing locally
4. Check GitHub Actions status page
5. Open an issue with logs and error messages

---

**Last Updated**: February 8, 2026  
**Workflow Version**: 1.0.0  
**Compatibility**: ERM-CE v4+
