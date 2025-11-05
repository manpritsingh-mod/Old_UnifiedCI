# Unified Jenkins CI/CD Shared Library - Comprehensive Documentation

## Part 2: Configuration & Setup

---

## Table of Contents

1. [Jenkins Setup](#jenkins-setup)
2. [Project Configuration](#project-configuration)
3. [Docker Registry Setup](#docker-registry-setup)
4. [Credentials Management](#credentials-management)
5. [Configuration Examples](#configuration-examples)

---

## Jenkins Setup

### 1. Add Shared Library to Jenkins

#### Location
Jenkins → Manage Jenkins → Configure System → Global Pipeline Libraries

#### Configuration
```
Name: My_UnifiedCI
Default version: main
Retrieval method: Modern SCM
  → Git
  → Project Repository: https://github.com/your-org/shared-library.git
  → Credentials: [your-git-credentials]
```

#### Load Implicitly
☐ Do NOT check "Load implicitly" - projects should explicitly import

#### Allow default version to be overridden
☑ Check this to allow projects to use specific versions

### 2. Jenkins Plugins Required

#### Essential Plugins
- **Pipeline**: Core pipeline functionality
- **Git**: Source code management
- **Docker Pipeline**: Docker support
- **Email Extension**: Email notifications
- **Allure**: Test reporting
- **Blue Ocean** (Optional): Better UI

#### Installation
```
Jenkins → Manage Jenkins → Manage Plugins → Available
Search and install each plugin
Restart Jenkins after installation
```

### 3. Jenkins Agent Configuration

#### Requirements
- Docker installed and running
- Access to Nexus Docker registry
- Sufficient disk space (10GB+ recommended)
- Network access to Git and Nexus

#### Agent Labels (Optional)
- `docker`: Agents with Docker
- `python`: Agents with Python pre-installed
- `java`: Agents with Java pre-installed

---

## Project Configuration

### File: ci-config.yaml

#### Location
**Project Root**: `<project-root>/ci-config.yaml`

#### Purpose
Defines pipeline behavior, tools, and settings

### Complete Configuration Reference

```yaml
# ============================================
# PROJECT CONFIGURATION
# ============================================

# Required: Project language
project_language: "python"  # Options: python, java-maven, java-gradle, react

# ============================================
# STAGE CONTROL
# ============================================

# Enable/Disable pipeline stages (default: true)
runUnitTests: true          # Run unit tests
runLintTests: true          # Run code quality checks
runFunctionalTests: true    # Enable functional test suite
runSmokeTests: true         # Run smoke tests
runSanityTests: true        # Run sanity tests
runRegressionTests: true    # Run regression tests

# ============================================
# TOOL SELECTION
# ============================================

# Testing tools by language
tool_for_unit_testing:
  python: pytest            # Options: pytest, unittest
  java: junit               # Options: junit, testng
  react: jest               # Options: jest, cypress

# Linting tools by language
tool_for_lint_testing:
  python: pylint            # Options: pylint, flake8, black
  java: checkstyle          # Options: checkstyle, spotbugs
  react: eslint             # Options: eslint, prettier

# ============================================
# DOCKER CONFIGURATION (REQUIRED)
# ============================================

nexus:
  url: "https://nexus.company.com:8082"      # Nexus URL with protocol
  registry: "nexus.company.com:8082"         # Registry without protocol
  credentials_id: "nexus-docker-creds"       # Jenkins credentials ID
  project: "dev"                             # Nexus project/repository

# Optional: Override default versions
python_version: "3.11"      # Default: 3.11
java-maven_version: "3.8.6" # Default: 3.8.6
java-gradle_version: "7.6.1"# Default: 7.6.1
react_version: "18"         # Default: 18 (Node.js version)

# ============================================
# NOTIFICATIONS
# ============================================

notifications:
  email:
    recipients:
      - "team@company.com"
      - "devops@company.com"
  slack:
    enabled: false          # Set to true when Slack is configured
    channel: "#builds"      # Slack channel for notifications
```

### Minimal Configuration (Python)

```yaml
project_language: "python"

nexus:
  url: "https://nexus.company.com:8082"
  registry: "nexus.company.com:8082"
  credentials_id: "nexus-docker-creds"
  project: "dev"

notifications:
  email:
    recipients:
      - "team@company.com"
```

### Configuration by Language

#### Python Project
```yaml
project_language: "python"
tool_for_unit_testing:
  python: pytest
tool_for_lint_testing:
  python: pylint
python_version: "3.11"
```

#### Java Maven Project
```yaml
project_language: "java-maven"
tool_for_unit_testing:
  java: junit
tool_for_lint_testing:
  java: checkstyle
java-maven_version: "3.8.6"
```

#### Java Gradle Project
```yaml
project_language: "java-gradle"
tool_for_unit_testing:
  java: junit
tool_for_lint_testing:
  java: spotbugs
java-gradle_version: "7.6.1"
```

#### React Project
```yaml
project_language: "react"
tool_for_unit_testing:
  react: jest
tool_for_lint_testing:
  react: eslint
react_version: "18"
```

---

## Docker Registry Setup

### Nexus Docker Registry Configuration

#### 1. Nexus Setup

**Location**: Nexus Repository Manager

**Steps**:
1. Create Docker (hosted) repository
2. Configure HTTP/HTTPS connector
3. Set up Docker Bearer Token Realm
4. Create deployment user

**Repository Structure**:
```
nexus.company.com:8082/
├── dev/
│   ├── python:3.8
│   ├── python:3.9
│   ├── python:3.11
│   ├── maven:3.8.6
│   ├── gradle:7.6.1
│   └── node:18
└── prod/
    └── [production images]
```

#### 2. Docker Images

**Required Images**:
- `nexus.company.com:8082/dev/python:3.11`
- `nexus.company.com:8082/dev/maven:3.8.6`
- `nexus.company.com:8082/dev/gradle:7.6.1`
- `nexus.company.com:8082/dev/node:18`

**How to Push Images**:
```bash
# Login to Nexus
docker login nexus.company.com:8082

# Tag image
docker tag python:3.11 nexus.company.com:8082/dev/python:3.11

# Push image
docker push nexus.company.com:8082/dev/python:3.11
```

#### 3. Network Configuration

**Firewall Rules**:
- Allow Jenkins agents to access Nexus (port 8082)
- Allow Docker pull operations
- Configure proxy if needed

**DNS**:
- Ensure `nexus.company.com` resolves correctly
- Add to `/etc/hosts` if needed

---

## Credentials Management

### Jenkins Credentials

#### Location
Jenkins → Manage Jenkins → Manage Credentials → Global

### Required Credentials

#### 1. Nexus Docker Credentials

**ID**: `nexus-docker-creds`
**Type**: Username with password
**Username**: `nexus-user`
**Password**: `[nexus-password]`
**Description**: Nexus Docker Registry credentials

**Usage**: Pulling Docker images from Nexus

#### 2. Git Credentials (if private repo)

**ID**: `git-credentials`
**Type**: Username with password OR SSH key
**Username**: `git-user`
**Password/Key**: `[git-password-or-key]`
**Description**: Git repository access

**Usage**: Checking out source code

#### 3. Email Credentials (if required)

**ID**: `email-credentials`
**Type**: Username with password
**Username**: `smtp-user`
**Password**: `[smtp-password]`
**Description**: SMTP server credentials

**Usage**: Sending email notifications

### How to Add Credentials

```
1. Go to Jenkins → Manage Jenkins → Manage Credentials
2. Click on "Global" domain
3. Click "Add Credentials"
4. Select credential type
5. Fill in details
6. Set ID (must match ci-config.yaml)
7. Click "OK"
```

### Security Best Practices

- ✅ Use Jenkins credentials store (encrypted)
- ✅ Never hardcode credentials in Jenkinsfile
- ✅ Use least privilege principle
- ✅ Rotate credentials regularly
- ✅ Audit credential usage
- ❌ Never commit credentials to Git
- ❌ Never log credentials in console

---

## Configuration Examples

### Example 1: Python Project with All Features

**File**: `ci-config.yaml`
```yaml
project_language: "python"

runUnitTests: true
runLintTests: true
runFunctionalTests: true
runSmokeTests: true
runSanityTests: true
runRegressionTests: true

tool_for_unit_testing:
  python: pytest

tool_for_lint_testing:
  python: pylint

nexus:
  url: "https://nexus.company.com:8082"
  registry: "nexus.company.com:8082"
  credentials_id: "nexus-docker-creds"
  project: "dev"

python_version: "3.11"

notifications:
  email:
    recipients:
      - "python-team@company.com"
      - "devops@company.com"
  slack:
    enabled: true
    channel: "#python-builds"
```

### Example 2: Java Maven Project (Minimal)

```yaml
project_language: "java-maven"

nexus:
  url: "https://nexus.company.com:8082"
  registry: "nexus.company.com:8082"
  credentials_id: "nexus-docker-creds"

notifications:
  email:
    recipients:
      - "java-team@company.com"
```

### Example 3: React Project (Custom Tools)

```yaml
project_language: "react"

runUnitTests: true
runLintTests: true
runFunctionalTests: false  # Disable functional tests

tool_for_unit_testing:
  react: jest

tool_for_lint_testing:
  react: eslint

nexus:
  url: "https://nexus.company.com:8082"
  registry: "nexus.company.com:8082"
  credentials_id: "nexus-docker-creds"

react_version: "18"

notifications:
  email:
    recipients:
      - "frontend-team@company.com"
```

---

**Continue to Part 3 for detailed pipeline execution...**
