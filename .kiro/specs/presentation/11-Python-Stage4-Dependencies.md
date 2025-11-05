# Slide 11: Python Pipeline - Stage 4: Install Dependencies

## Stage 4: Install Dependencies in Virtual Environment

### Visual: Dependency Installation Process

```
┌──────────────────────────────────────────────────────────────────┐
│              STAGE 4: INSTALL DEPENDENCIES                        │
├──────────────────────────────────────────────────────────────────┤
│                                                                   │
│  PURPOSE:                                                         │
│  Create isolated Python environment and install all dependencies │
│                                                                   │
│  PROCESS FLOW:                                                    │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │                                                             │ │
│  │  Step 1: Create Virtual Environment                         │ │
│  │  ┌─────────────────────────────────────────┐              │ │
│  │  │  $ python3 -m venv venv                 │              │ │
│  │  │                                          │              │ │
│  │  │  Creates:                                │              │ │
│  │  │  venv/                                   │              │ │
│  │  │  ├── bin/                                │              │ │
│  │  │  │   ├── python                          │              │ │
│  │  │  │   ├── pip                             │              │ │
│  │  │  │   └── activate                        │              │ │
│  │  │  ├── lib/                                │              │ │
│  │  │  └── include/                            │              │ │
│  │  └─────────────────────────────────────────┘              │ │
│  │                      │                                      │ │
│  │                      ▼                                      │ │
│  │  Step 2: Install Dependencies                               │ │
│  │  ┌─────────────────────────────────────────┐              │ │
│  │  │  $ venv/bin/pip install -r               │              │ │
│  │  │    requirements.txt                      │              │ │
│  │  │                                          │              │ │
│  │  │  Installing:                             │              │ │
│  │  │  • pytest>=7.4.0                         │              │ │
│  │  │  • pytest-cov>=4.1.0                     │              │ │
│  │  │  • pylint>=2.17.0                        │              │ │
│  │  │  • black>=23.7.0                         │              │ │
│  │  │  • flake8>=6.0.0                         │              │ │
│  │  │                                          │              │ │
│  │  │  + All transitive dependencies           │              │ │
│  │  └─────────────────────────────────────────┘              │ │
│  │                      │                                      │ │
│  │                      ▼                                      │ │
│  │  Step 3: Verify Installation                                │ │
│  │  ┌─────────────────────────────────────────┐              │ │
│  │  │  $ venv/bin/pip list                     │              │ │
│  │  │                                          │              │ │
│  │  │  Package         Version                 │              │ │
│  │  │  ─────────────   ─────────               │              │ │
│  │  │  pytest          7.4.0                   │              │ │
│  │  │  pytest-cov      4.1.0                   │              │ │
│  │  │  pylint          2.17.0                  │              │ │
│  │  │  black           23.7.0                  │              │ │
│  │  │  flake8          6.0.0                   │              │ │
│  │  │  ...             ...                     │              │ │
│  │  └─────────────────────────────────────────┘              │ │
│  │                                                             │ │
│  └────────────────────────────────────────────────────────────┘ │
│                                                                   │
│  TOOLS USED:                                                      │
│  • Python venv module                                             │
│  • pip (Python package installer)                                │
│  • requirements.txt                                               │
│                                                                   │
│  CODE EXECUTED:                                                   │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  // Create virtual environment                              │ │
│  │  sh script: PythonScript.createVirtualEnvCommand()          │ │
│  │  // Returns: "python3 -m venv venv"                         │ │
│  │                                                             │ │
│  │  // Install dependencies                                    │ │
│  │  if (fileExists('requirements.txt')) {                      │ │
│  │      sh script: PythonScript.venvPipInstallLinuxCommand()  │ │
│  │      // Returns: "venv/bin/pip install -r requirements.txt" │ │
│  │  }                                                          │ │
│  └────────────────────────────────────────────────────────────┘ │
│                                                                   │
│  DEPENDENCIES INSTALLED:                                          │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  TESTING:                                                   │ │
│  │  • pytest - Test framework                                  │ │
│  │  • pytest-cov - Coverage plugin                             │ │
│  │                                                             │ │
│  │  CODE QUALITY:                                              │ │
│  │  • pylint - Linter                                          │ │
│  │  • black - Code formatter                                   │ │
│  │  • flake8 - Style checker                                   │ │
│  │                                                             │ │
│  │  TRANSITIVE:                                                │ │
│  │  • coverage - Coverage measurement                          │ │
│  │  • pluggy - Plugin system                                   │ │
│  │  • iniconfig - Config parser                                │ │
│  │  • + 20+ more packages                                      │ │
│  └────────────────────────────────────────────────────────────┘ │
│                                                                   │
│  DURATION: ~20-40 seconds                                         │
│  (Depends on number of packages and network speed)               │
│                                                                   │
│  WHY VIRTUAL ENVIRONMENT?                                         │
│  ✅ Isolated from system Python                                  │
│  ✅ No conflicts with other projects                             │
│  ✅ Reproducible builds                                          │
│  ✅ Easy cleanup                                                 │
│  ✅ Matches local development                                    │
│                                                                   │
│  SUCCESS CRITERIA:                                                │
│  ✅ Virtual environment created                                  │
│  ✅ All packages installed successfully                          │
│  ✅ No dependency conflicts                                      │
│  ✅ Correct versions installed                                   │
│                                                                   │
│  POSSIBLE FAILURES:                                               │
│  ❌ requirements.txt not found                                   │
│  ❌ Package not available in PyPI                                │
│  ❌ Version conflicts                                            │
│  ❌ Network issues                                               │
│  ❌ Disk space full                                              │
│                                                                   │
└──────────────────────────────────────────────────────────────────┘
```

---

### Speaker Notes:
"Stage 4 creates a clean virtual environment and installs all dependencies. This isolation is critical - it ensures our build doesn't interfere with system Python or other projects. We install pytest for testing, pylint for code quality, and coverage tools. Takes 20-40 seconds depending on the number of packages."
