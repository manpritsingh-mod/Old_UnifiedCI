# UnifiedCI Shared Library - Complete Documentation

## 📋 Table of Contents
- [Overview](#overview)
- [Architecture](#architecture)
- [Repository Structure](#repository-structure)
- [Prerequisites](#prerequisites)
- [Quick Start Guide](#quick-start-guide)
- [Available Templates](#available-templates)
- [Pipeline Stages Deep Dive](#pipeline-stages-deep-dive)
- [Configuration Guide](#configuration-guide)
- [Tool Specifications](#tool-specifications)
- [Troubleshooting](#troubleshooting)
- [FAQ](#faq)
- [Support](#support)

---

## 🎯 Overview

### What is UnifiedCI Shared Library?

UnifiedCI is a centralized Jenkins Shared Library that provides reusable, standardized CI/CD pipeline templates for multiple project types. It eliminates code duplication, ensures consistency across projects, and simplifies pipeline maintenance.

### Why Was It Created?

**Problems We Solved:**
- **Code Duplication**: Every project was writing the same Jenkinsfile logic
- **Maintenance Overhead**: Bug fixes had to be applied to each project individually
- **Inconsistency**: Different teams using different tools and approaches
- **Quality Gaps**: Some projects skipped important quality gates

**Solution Benefits:**
- ✅ **90% reduction** in Jenkinsfile code per project
- ✅ **Single point of maintenance** - update once, benefit everywhere
- ✅ **Enforced standards** - all projects follow the same quality gates
- ✅ **Faster onboarding** - new projects setup in minutes

---

## 🏗️ Architecture

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    GITHUB REPOSITORY                         │
│              UnifiedCI Shared Library Repo                   │
└────────────────────┬────────────────────────────────────────┘
                     │
                     │ @Library Import
                     │
┌────────────────────▼────────────────────────────────────────┐
│                  JENKINS SERVER                              │
│  ┌──────────────────────────────────────────────────────┐  │
│  │              Project Jenkinsfile                      │  │
│  │  @Library('UnifiedCI@main') _                        │  │
│  │  pythonTemplate()                                    │  │
│  └──────────────────────────────────────────────────────┘  │
│                          │                                   │
│                          ▼                                   │
│  ┌──────────────────────────────────────────────────────┐  │
│  │          SHARED LIBRARY TEMPLATE                      │  │
│  │  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐ │  │
│  │  │Checkout │→ │  Lint   │→ │  Test   │→ │  Build  │ │  │
│  │  └─────────┘  └─────────┘  └─────────┘  └─────────┘ │  │
│  │       │            │             │            │        │  │
│  │       ▼            ▼             ▼            ▼        │  │
│  │    GitHub      Pylint/       pytest/      setup.py/   │  │
│  │               ESLint       JUnit/Jest      Maven      │  │
│  └──────────────────────────────────────────────────────┘  │
│                          │                                   │
│                          ▼                                   │
│  ┌──────────────────────────────────────────────────────┐  │
│  │           DEPLOYMENT & NOTIFICATIONS                  │  │
│  │  Allure Reports │ Email │ Slack │ MS Teams           │  │
│  └──────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────┘
```

### Pipeline Flow

```
┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
│ Checkout │ → │   Lint   │ → │   Test   │ → │  Build   │ → │  Deploy  │
└──────────┘    └──────────┘    └──────────┘    └──────────┘    └──────────┘
     │               │                │                │               │
     ▼               ▼                ▼                ▼               ▼
  Get Code    Code Quality    Unit Testing    Compile &      Deploy to
  from Git    Validation      Coverage         Package        Environment
```

---

## 📁 Repository Structure

### Shared Library Repository Layout

```
UnifiedCI-Shared-Library/
│
├── vars/                              # Global variables and template definitions
│   ├── pythonTemplate.groovy         # Python project pipeline template
│   ├── mavenTemplate.groovy          # Maven (Java) project template
│   ├── gradleTemplate.groovy         # Gradle (Java/Kotlin) template
│   ├── reactTemplate.groovy          # React/Node.js template
│   ├── checkoutStage.groovy          # Reusable checkout stage
│   ├── lintStage.groovy              # Reusable lint stage
│   ├── testStage.groovy              # Reusable test stage
│   ├── buildStage.groovy             # Reusable build stage
│   └── deployStage.groovy            # Reusable deployment stage
│
├── src/                               # Groovy source files (classes)
│   └── com/
│       └── company/
│           └── jenkins/
│               ├── BuildUtils.groovy          # Build utility functions
│               ├── TestUtils.groovy           # Testing utility functions
│               ├── NotificationUtils.groovy   # Notification helpers
│               └── ConfigParser.groovy        # Configuration parsing
│
├── resources/                         # Configuration files and scripts
│   ├── config/
│   │   ├── checkstyle.xml            # Java Checkstyle rules
│   │   ├── pylint.rc                 # Python Pylint configuration
│   │   ├── eslint.config.js          # JavaScript ESLint rules
│   │   └── sonar-project.properties  # SonarQube settings
│   │
│   └── scripts/
│       ├── deploy.sh                 # Deployment helper script
│       └── notification.sh           # Notification script
│
├── docs/                              # Documentation
│   ├── README.md                     # This file
│   ├── ARCHITECTURE.md               # Architecture details
│   └── EXAMPLES.md                   # Usage examples
│
└── Jenkinsfile                        # Self-testing pipeline for the library
```

### Consumer Project Structure (Your Project)

```
your-project/
│
├── src/                               # Your source code
│   └── main/
│       └── python/                    # (or java/, js/, etc.)
│
├── tests/                             # Your test files
│   └── test_*.py
│
├── Jenkinsfile                        # Your pipeline definition (2 lines!)
│   @Library('UnifiedCI@main') _
│   pythonTemplate()
│
├── requirements.txt                   # Python dependencies
├── setup.py                           # Python package setup
└── .jenkins-config.yml               # (Optional) Custom configuration
```

---

## 🔧 Prerequisites

### Jenkins Configuration

**Required Jenkins Plugins:**
```
- Pipeline: Groovy (workflow-cps)
- Pipeline: Shared Groovy Libraries
- Git Plugin
- GitHub Plugin
- Allure Jenkins Plugin
- Email Extension Plugin
- Slack Notification Plugin (optional)
- Blue Ocean (optional, for better UI)
```

**To Install Plugins:**
1. Jenkins → Manage Jenkins → Manage Plugins
2. Search and install required plugins
3. Restart Jenkins

### Jenkins Shared Library Configuration

**Setup Steps:**

1. **Navigate to Jenkins Configuration**
   - Jenkins → Manage Jenkins → Configure System
   - Scroll to "Global Pipeline Libraries"

2. **Add New Library**
   ```
   Name: UnifiedCI
   Default version: main
   Load implicitly: ☐ (unchecked)
   Allow default version to be overridden: ☑ (checked)
   Include @Library changes in job recent changes: ☑ (checked)
   
   Retrieval method: Modern SCM
   Source Code Management: Git
   Project Repository: https://github.com/your-org/UnifiedCI-Shared-Library.git
   Credentials: (Select GitHub credentials with read access)
   ```

3. **Save Configuration**

### Jenkins Credentials Setup

**Required Credentials:**

1. **GitHub Credentials** (ID: `github-credentials`)
   - Type: Username with password or SSH key
   - Used for: Checking out code from repositories

2. **Deployment Credentials** (ID: `deploy-credentials`)
   - Type: Username with password
   - Used for: Deploying to servers

3. **Email Configuration**
   - Jenkins → Manage Jenkins → Configure System
   - E-mail Notification section
   - SMTP server: smtp.gmail.com (or your SMTP)
   - Use SMTP Authentication: Yes
   - Add credentials

4. **Slack Token** (Optional, ID: `slack-token`)
   - Type: Secret text
   - Used for: Slack notifications

---

## 🚀 Quick Start Guide

### Step 1: Create Jenkinsfile in Your Project

Create a file named `Jenkinsfile` in your project root:

**For Python Projects:**
```groovy
@Library('UnifiedCI@main') _

pythonTemplate()
```

**For Maven Projects:**
```groovy
@Library('UnifiedCI@main') _

mavenTemplate()
```

**For Gradle Projects:**
```groovy
@Library('UnifiedCI@main') _

gradleTemplate()
```

**For React Projects:**
```groovy
@Library('UnifiedCI@main') _

reactTemplate()
```

### Step 2: Configure Jenkins Job

1. **Create New Item**
   - Jenkins → New Item
   - Enter project name
   - Select "Pipeline"
   - Click OK

2. **Configure Pipeline**
   - Scroll to "Pipeline" section
   - Definition: Pipeline script from SCM
   - SCM: Git
   - Repository URL: Your project's Git URL
   - Credentials: Select GitHub credentials
   - Branch: */main (or your default branch)
   - Script Path: Jenkinsfile

3. **Save and Build**
   - Click "Save"
   - Click "Build Now"

### Step 3: Verify Pipeline Execution

Watch the pipeline execute through stages:
```
✓ Checkout    - Code fetched from Git
✓ Lint        - Code quality checks passed
✓ Test        - Unit tests executed
✓ Build       - Artifacts created
✓ Deploy      - Deployed successfully
```

---

## 📦 Available Templates

### 1. Python Template

**File:** `vars/pythonTemplate.groovy`

**Usage:**
```groovy
@Library('UnifiedCI@main') _

pythonTemplate(
    pythonVersion: '3.9',           // Python version (default: 3.9)
    requirementsFile: 'requirements.txt',  // Dependencies file
    testCommand: 'pytest',          // Test command
    coverageThreshold: 80,          // Minimum coverage %
    deployEnvironment: 'dev'        // Target environment
)
```

**Tools Used:**
- **Lint:** Pylint, Flake8, Black
- **Test:** pytest, coverage.py
- **Build:** setuptools, wheel
- **Deploy:** Custom deploy scripts

**Expected Project Structure:**
```
project/
├── src/
├── tests/
├── requirements.txt
├── setup.py
└── Jenkinsfile
```

---

### 2. Maven Template

**File:** `vars/mavenTemplate.groovy`

**Usage:**
```groovy
@Library('UnifiedCI@main') _

mavenTemplate(
    jdkVersion: '11',               // Java version
    mavenGoals: 'clean package',    // Maven goals
    runTests: true,                 // Execute tests
    sonarAnalysis: false,           // SonarQube scan
    deployEnvironment: 'staging'
)
```

**Tools Used:**
- **Lint:** Checkstyle, PMD, SpotBugs
- **Test:** JUnit 5, Mockito, JaCoCo
- **Build:** Maven
- **Deploy:** Maven deploy plugin

**Expected Project Structure:**
```
project/
├── src/
│   ├── main/java/
│   └── test/java/
├── pom.xml
└── Jenkinsfile
```

---

### 3. Gradle Template

**File:** `vars/gradleTemplate.groovy`

**Usage:**
```groovy
@Library('UnifiedCI@main') _

gradleTemplate(
    jdkVersion: '17',
    gradleTasks: 'clean build',
    runTests: true,
    publishArtifacts: true
)
```

**Tools Used:**
- **Lint:** Checkstyle, Ktlint (for Kotlin)
- **Test:** JUnit, TestNG, Spock
- **Build:** Gradle
- **Deploy:** Gradle publish plugin

---

### 4. React Template

**File:** `vars/reactTemplate.groovy`

**Usage:**
```groovy
@Library('UnifiedCI@main') _

reactTemplate(
    nodeVersion: '18',
    packageManager: 'npm',          // npm or yarn
    buildCommand: 'npm run build',
    testCommand: 'npm test',
    deployTarget: 's3'              // s3, nginx, etc.
)
```

**Tools Used:**
- **Lint:** ESLint, Prettier
- **Test:** Jest, React Testing Library
- **Build:** Webpack/Vite
- **Deploy:** S3, Nginx, or custom

---

## 🔍 Pipeline Stages Deep Dive

### Stage 1: Checkout

**Purpose:** Retrieve source code from version control system

**Implementation:** `vars/checkoutStage.groovy`

```groovy
def call() {
    stage('Checkout') {
        echo "🔄 Checking out code from repository..."
        checkout scm
        
        // Get commit information
        env.GIT_COMMIT_HASH = sh(
            returnStdout: true,
            script: 'git rev-parse --short HEAD'
        ).trim()
        
        env.GIT_AUTHOR = sh(
            returnStdout: true,
            script: 'git log -1 --pretty=%an'
        ).trim()
        
        echo "✅ Checkout completed. Commit: ${env.GIT_COMMIT_HASH}"
    }
}
```

**What It Does:**
1. Clones repository from GitHub/GitLab
2. Checks out specified branch or commit
3. Captures commit metadata (hash, author, message)
4. Sets environment variables for downstream stages

**Configuration:**
- Repository URL: Configured in Jenkins job
- Branch: Specified in job or Jenkinsfile
- Credentials: Stored in Jenkins credentials

---

### Stage 2: Lint / Code Quality

**Purpose:** Enforce coding standards and detect potential issues

**Implementation:** `vars/lintStage.groovy`

```groovy
def call(Map config) {
    stage('Lint') {
        echo "🔍 Running code quality checks..."
        
        switch(config.projectType) {
            case 'python':
                runPythonLint()
                break
            case 'java':
                runJavaLint()
                break
            case 'javascript':
                runJavaScriptLint()
                break
        }
    }
}

def runPythonLint() {
    sh '''
        echo "Running Pylint..."
        pylint src/ --rcfile=.pylintrc || true
        
        echo "Running Flake8..."
        flake8 src/ --max-line-length=120
        
        echo "Running Black (formatter check)..."
        black --check src/
    '''
}
```

**Tools by Project Type:**

#### Python Projects
```bash
# Pylint - Comprehensive Python linter
pylint src/ --rcfile=.pylintrc

# Flake8 - Style guide enforcement
flake8 src/ --max-line-length=120 --ignore=E203,W503

# Black - Code formatter
black --check src/
```

**Configuration File:** `resources/config/pylint.rc`

#### Maven Projects
```bash
# Checkstyle - Java style checker
mvn checkstyle:check

# PMD - Source code analyzer
mvn pmd:check

# SpotBugs - Bug detector
mvn spotbugs:check
```

**Configuration File:** `resources/config/checkstyle.xml`

#### Gradle Projects
```bash
# Checkstyle
gradle checkstyleMain checkstyleTest

# Ktlint (for Kotlin)
gradle ktlintCheck
```

#### React Projects
```bash
# ESLint - JavaScript linter
npm run lint

# Prettier - Code formatter
npm run format:check
```

**Configuration File:** `resources/config/eslint.config.js`

**Failure Handling:**
- Critical issues → Pipeline FAILS
- Warnings → Pipeline continues but marks build as UNSTABLE
- Reports published to Jenkins for review

---

### Stage 3: Unit Testing

**Purpose:** Execute automated tests and measure code coverage

**Implementation:** `vars/testStage.groovy`

```groovy
def call(Map config) {
    stage('Test') {
        echo "🧪 Running unit tests..."
        
        try {
            switch(config.projectType) {
                case 'python':
                    runPythonTests(config)
                    break
                case 'java':
                    runJavaTests(config)
                    break
                case 'javascript':
                    runJavaScriptTests(config)
                    break
            }
            
            // Publish test results
            publishTestResults(config)
            
        } catch (Exception e) {
            currentBuild.result = 'FAILURE'
            error("Tests failed: ${e.message}")
        }
    }
}

def runPythonTests(Map config) {
    sh """
        pytest tests/ \
            --cov=src \
            --cov-report=html:coverage-report \
            --cov-report=xml:coverage.xml \
            --junitxml=test-results.xml \
            --cov-fail-under=${config.coverageThreshold ?: 80}
    """
}
```

**Tools by Project Type:**

#### Python
```bash
# pytest - Testing framework
pytest tests/ \
    --cov=src \
    --cov-report=html \
    --cov-report=xml \
    --junitxml=test-results.xml

# unittest (alternative)
python -m unittest discover tests/
```

**Coverage Threshold:** 80% (configurable)

#### Maven
```bash
# JUnit 5 + JaCoCo
mvn test jacoco:report

# Coverage check
mvn jacoco:check -Dcoverage.minimum=0.80
```

**Reports:** target/site/jacoco/index.html

#### Gradle
```bash
# JUnit tests
gradle test

# JaCoCo coverage
gradle jacocoTestReport
gradle jacocoTestCoverageVerification
```

#### React
```bash
# Jest tests
npm test -- --coverage --watchAll=false

# Coverage threshold
npm test -- --coverage --coverageThreshold='{"global":{"lines":80}}'
```

**Test Results Publishing:**
```groovy
junit 'test-results.xml'
publishHTML([
    reportDir: 'coverage-report',
    reportFiles: 'index.html',
    reportName: 'Coverage Report'
])
```

---

### Stage 4: Build

**Purpose:** Compile code and create deployable artifacts

**Implementation:** `vars/buildStage.groovy`

```groovy
def call(Map config) {
    stage('Build') {
        echo "🔨 Building project..."
        
        switch(config.projectType) {
            case 'python':
                buildPython(config)
                break
            case 'java':
                buildJava(config)
                break
            case 'javascript':
                buildJavaScript(config)
                break
        }
        
        archiveArtifacts(config)
    }
}

def buildPython(Map config) {
    sh '''
        python setup.py sdist bdist_wheel
    '''
}
```

**Build Process by Project Type:**

#### Python
```bash
# Create distributable packages
python setup.py sdist bdist_wheel

# Output: dist/your-package-1.0.0.tar.gz
#         dist/your_package-1.0.0-py3-none-any.whl
```

**Artifact:** `.whl` or `.tar.gz` file

#### Maven
```bash
# Clean and package
mvn clean package -DskipTests

# Output: target/your-app-1.0.0.jar
```

**Artifact:** `.jar` or `.war` file

#### Gradle
```bash
# Build project
gradle clean build -x test

# Output: build/libs/your-app-1.0.0.jar
```

#### React
```bash
# Production build
npm run build

# Output: build/ directory with static assets
```

**Artifact Archiving:**
```groovy
archiveArtifacts artifacts: 'dist/*.whl, dist/*.tar.gz',
                 fingerprint: true
```

---

### Stage 5: Deploy & Notifications

**Purpose:** Deploy artifacts and notify stakeholders

**Implementation:** `vars/deployStage.groovy`

```groovy
def call(Map config) {
    stage('Deploy') {
        echo "🚀 Deploying to ${config.environment}..."
        
        // Deploy based on environment
        if (config.environment == 'production') {
            input message: 'Deploy to production?',
                  ok: 'Deploy'
        }
        
        deploy(config)
        publishReports()
        sendNotifications(config)
    }
}
```

**Deployment Targets:**

#### Development Environment
- **Trigger:** Automatic on commit to `develop` branch
- **Approval:** None required
- **Target:** Dev server or Docker container

#### Staging Environment
- **Trigger:** Automatic on commit to `staging` branch
- **Approval:** None required
- **Target:** Staging server

#### Production Environment
- **Trigger:** Manual or on tag creation
- **Approval:** Required (manual input)
- **Target:** Production servers

**Allure Reports Integration:**
```groovy
allure([
    includeProperties: false,
    jdk: '',
    properties: [],
    reportBuildPolicy: 'ALWAYS',
    results: [[path: 'allure-results']]
])
```

**Email Notifications:**
```groovy
emailext (
    subject: "Pipeline ${currentBuild.result}: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
    body: """
        <p>Build Status: ${currentBuild.result}</p>
        <p>Project: ${env.JOB_NAME}</p>
        <p>Build Number: ${env.BUILD_NUMBER}</p>
        <p>Commit: ${env.GIT_COMMIT_HASH}</p>
        <p>Author: ${env.GIT_AUTHOR}</p>
        <p><a href="${env.BUILD_URL}">View Build</a></p>
    """,
    to: 'team@company.com',
    mimeType: 'text/html'
)
```

**Slack Notifications:**
```groovy
slackSend(
    channel: '#devops-alerts',
    color: currentBuild.result == 'SUCCESS' ? 'good' : 'danger',
    message: "Pipeline ${currentBuild.result}: ${env.JOB_NAME} #${env.BUILD_NUMBER}"
)
```

---

## ⚙️ Configuration Guide

### Custom Configuration File

Create `.jenkins-config.yml` in your project root:

```yaml
# .jenkins-config.yml

# Project settings
project:
  type: python
  name: my-awesome-app
  version: 1.0.0

# Build configuration
build:
  python_version: "3.9"
  requirements_file: requirements.txt
  
# Testing configuration
testing:
  framework: pytest
  coverage_threshold: 85
  test_directory: tests/
  
# Linting configuration
linting:
  enabled: true
  tools:
    - pylint
    - flake8
    - black
  fail_on_error: true

# Deployment configuration
deployment:
  environments:
    dev:
      auto_deploy: true
      server: dev.company.com
    staging:
      auto_deploy: false
      server: staging.company.com
    production:
      auto_deploy: false
      requires_approval: true
      server: prod.company.com

# Notifications
notifications:
  email:
    enabled: true
    recipients:
      - team@company.com
      - devops@company.com
  slack:
    enabled: true
    channel: "#build-notifications"
```

### Using Custom Configuration in Template

```groovy
@Library('UnifiedCI@main') _

// Load custom config
def config = readYaml file: '.jenkins-config.yml'

pythonTemplate(
    pythonVersion: config.build.python_version,
    coverageThreshold: config.testing.coverage_threshold,
    deployEnvironment: 'dev'
)
```

---

## 🛠️ Tool Spec