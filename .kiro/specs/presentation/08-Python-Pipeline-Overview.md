# Slide 8: Python Pipeline - Complete Flow

## Python CI/CD Pipeline Stages

### Visual: Complete Pipeline Flow

```
┌──────────────────────────────────────────────────────────────────┐
│                    PYTHON PIPELINE - COMPLETE FLOW                │
├──────────────────────────────────────────────────────────────────┤
│                                                                   │
│  START                                                            │
│    │                                                              │
│    ▼                                                              │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │ STAGE 1: CHECKOUT                                        │    │
│  │ • Clone code from Git repository                         │    │
│  │ • Tools: Git                                             │    │
│  │ • Duration: ~5 seconds                                   │    │
│  └────────────────────────┬─────────────────────────────────┘    │
│                           │                                       │
│                           ▼                                       │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │ STAGE 2: PULL DOCKER IMAGE                               │    │
│  │ • Pull Python Docker image from Nexus                    │    │
│  │ • Tools: Docker, Nexus Registry                          │    │
│  │ • Image: python:3.11                                     │    │
│  │ • Duration: ~15-30 seconds (cached: ~5 seconds)          │    │
│  └────────────────────────┬─────────────────────────────────┘    │
│                           │                                       │
│                           ▼                                       │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │ STAGE 3: SETUP                                           │    │
│  │ • Verify Python & pip versions                           │    │
│  │ • Set environment variables                              │    │
│  │ • Tools: Python 3.11, pip                                │    │
│  │ • Duration: ~5 seconds                                   │    │
│  └────────────────────────┬─────────────────────────────────┘    │
│                           │                                       │
│                           ▼                                       │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │ STAGE 4: INSTALL DEPENDENCIES                            │    │
│  │ • Create virtual environment (venv)                      │    │
│  │ • Install packages from requirements.txt                 │    │
│  │ • Tools: pip, virtualenv                                 │    │
│  │ • Packages: pytest, pylint, coverage                     │    │
│  │ • Duration: ~20-40 seconds                               │    │
│  └────────────────────────┬─────────────────────────────────┘    │
│                           │                                       │
│                           ▼                                       │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │ STAGE 5: LINT (Code Quality)                            │    │
│  │ • Run pylint for code quality checks                     │    │
│  │ • Tools: pylint, flake8 (optional)                       │    │
│  │ • Checks: Style, errors, warnings                        │    │
│  │ • Result: UNSTABLE if issues found (continues)           │    │
│  │ • Duration: ~10-15 seconds                               │    │
│  └────────────────────────┬─────────────────────────────────┘    │
│                           │                                       │
│                           ▼                                       │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │ STAGE 6: BUILD                                           │    │
│  │ • Build Python package (setup.py)                        │    │
│  │ • Tools: setuptools                                      │    │
│  │ • Creates: Distributable package                         │    │
│  │ • Duration: ~5-10 seconds                                │    │
│  └────────────────────────┬─────────────────────────────────┘    │
│                           │                                       │
│                           ▼                                       │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │ STAGE 7: TEST EXECUTION (PARALLEL)                       │    │
│  │                                                           │    │
│  │  ┌──────────────────┐        ┌──────────────────┐       │    │
│  │  │  UNIT TESTS      │        │ FUNCTIONAL TESTS │       │    │
│  │  │                  │        │                  │       │    │
│  │  │  • pytest        │        │  • Smoke Tests   │       │    │
│  │  │  • Coverage      │        │  • Sanity Tests  │       │    │
│  │  │  • 50+ tests     │        │  • Regression    │       │    │
│  │  │                  │        │    Tests         │       │    │
│  │  │  Duration: ~15s  │        │  Duration: ~20s  │       │    │
│  │  └──────────────────┘        └──────────────────┘       │    │
│  │                                                           │    │
│  │  Total Duration: ~20 seconds (parallel execution)        │    │
│  └────────────────────────┬─────────────────────────────────┘    │
│                           │                                       │
│                           ▼                                       │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │ STAGE 8: GENERATE REPORTS                                │    │
│  │ • Generate Allure test reports                           │    │
│  │ • Send email notifications                               │    │
│  │ • Tools: Allure, Email plugin                            │    │
│  │ • Duration: ~10 seconds                                  │    │
│  └────────────────────────┬─────────────────────────────────┘    │
│                           │                                       │
│                           ▼                                       │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │ STAGE 9: CLEANUP                                         │    │
│  │ • Remove virtual environment                             │    │
│  │ • Clean temporary files                                  │    │
│  │ • Duration: ~5 seconds                                   │    │
│  └────────────────────────┬─────────────────────────────────┘    │
│                           │                                       │
│                           ▼                                       │
│                         END                                       │
│                                                                   │
│  TOTAL PIPELINE DURATION: ~2-3 minutes                           │
│                                                                   │
└──────────────────────────────────────────────────────────────────┘
```

### Key Metrics:
- **Total Stages:** 9
- **Parallel Execution:** Yes (Tests)
- **Average Duration:** 2-3 minutes
- **Success Rate:** 95%+

---

### Speaker Notes:
"This is the complete Python pipeline flow. Nine stages execute automatically, with tests running in parallel to save time. The entire pipeline completes in 2-3 minutes, giving developers fast feedback."
