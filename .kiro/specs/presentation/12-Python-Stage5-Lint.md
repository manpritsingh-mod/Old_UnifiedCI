# Slide 12: Python Pipeline - Stage 5: Lint (Code Quality)

## Stage 5: Code Quality Checks with Pylint

### Visual: Linting Process

```
┌──────────────────────────────────────────────────────────────────┐
│                  STAGE 5: LINT (CODE QUALITY)                     │
├──────────────────────────────────────────────────────────────────┤
│                                                                   │
│  PURPOSE:                                                         │
│  Analyze code quality, find bugs, enforce coding standards       │
│                                                                   │
│  PROCESS FLOW:                                                    │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │                                                             │ │
│  │  Source Code                                                │ │
│  │  ┌─────────────────────────────────────────┐              │ │
│  │  │  src/calculator/                        │              │ │
│  │  │  ├── __init__.py                        │              │ │
│  │  │  ├── calculator.py                      │              │ │
│  │  │  └── app.py                             │              │ │
│  │  └─────────────────────────────────────────┘              │ │
│  │                      │                                      │ │
│  │                      │ Analyze                             │ │
│  │                      ▼                                      │ │
│  │  Pylint Analysis                                            │ │
│  │  ┌─────────────────────────────────────────┐              │ │
│  │  │  $ venv/bin/pylint **/*.py              │              │ │
│  │  │    --output-format=text                 │              │ │
│  │  │                                          │              │ │
│  │  │  Checking:                               │              │ │
│  │  │  • Code style (PEP 8)                    │              │ │
│  │  │  • Potential bugs                        │              │ │
│  │  │  • Code smells                           │              │ │
│  │  │  • Unused variables                      │              │ │
│  │  │  • Missing docstrings                    │              │ │
│  │  │  • Line length                           │              │ │
│  │  │  • Naming conventions                    │              │ │
│  │  └─────────────────────────────────────────┘              │ │
│  │                      │                                      │ │
│  │                      ▼                                      │ │
│  │  Lint Results                                               │ │
│  │  ┌─────────────────────────────────────────┐              │ │
│  │  │  ************* Module calculator         │              │ │
│  │  │  calculator.py:12:4: C0103: Constant     │              │ │
│  │  │    name "pi_value" doesn't conform to    │              │ │
│  │  │    UPPER_CASE naming style               │              │ │
│  │  │                                          │              │ │
│  │  │  calculator.py:35:4: C0301: Line too     │              │ │
│  │  │    long (120/100)                        │              │ │
│  │  │                                          │              │ │
│  │  │  calculator.py:38:4: C0116: Missing      │              │ │
│  │  │    function docstring                    │              │ │
│  │  │                                          │              │ │
│  │  │  calculator.py:52:8: W0612: Unused       │              │ │
│  │  │    variable 'unused_var'                 │              │ │
│  │  │                                          │              │ │
│  │  │  Your code has been rated at 7.5/10      │              │ │
│  │  └─────────────────────────────────────────┘              │ │
│  │                                                             │ │
│  └────────────────────────────────────────────────────────────┘ │
│                                                                   │
│  TOOLS USED:                                                      │
│  • pylint (Primary linter)                                        │
│  • flake8 (Optional alternative)                                 │
│  • black (Optional formatter check)                              │
│                                                                   │
│  CONFIGURATION (.pylintrc):                                       │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  [MASTER]                                                   │ │
│  │  init-hook='import sys; sys.path.append("src")'            │ │
│  │                                                             │ │
│  │  [MESSAGES CONTROL]                                         │ │
│  │  disable=                                                   │ │
│  │      missing-module-docstring,                              │ │
│  │      too-few-public-methods                                 │ │
│  │                                                             │ │
│  │  [FORMAT]                                                   │ │
│  │  max-line-length=100                                        │ │
│  │  max-module-lines=1000                                      │ │
│  └────────────────────────────────────────────────────────────┘ │
│                                                                   │
│  CODE EXECUTED:                                                   │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  def lintTool = config.tool_for_lint_testing?.python        │ │
│  │                 ?: 'pylint'                                 │ │
│  │                                                             │ │
│  │  try {                                                      │ │
│  │      sh script: PythonScript.venvLintLinuxCommand(lintTool)│ │
│  │      // Returns: "venv/bin/pylint **/*.py"                  │ │
│  │      return 'SUCCESS'                                       │ │
│  │  } catch (Exception e) {                                    │ │
│  │      // Violations found, but continue pipeline            │ │
│  │      currentBuild.result = 'UNSTABLE'                       │ │
│  │      return 'UNSTABLE'                                      │ │
│  │  }                                                          │ │
│  └────────────────────────────────────────────────────────────┘ │
│                                                                   │
│  WHAT PYLINT CHECKS:                                              │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  CONVENTION (C): Coding standard violations                 │ │
│  │  • Naming conventions                                       │ │
│  │  • Line length                                              │ │
│  │  • Whitespace                                               │ │
│  │                                                             │ │
│  │  WARNING (W): Potential issues                              │ │
│  │  • Unused variables                                         │ │
│  │  • Unused imports                                           │ │
│  │  • Redefined variables                                      │ │
│  │                                                             │ │
│  │  ERROR (E): Probable bugs                                   │ │
│  │  • Undefined variables                                      │ │
│  │  • Invalid syntax                                           │ │
│  │  • Import errors                                            │ │
│  │                                                             │ │
│  │  REFACTOR (R): Code smells                                  │ │
│  │  • Too many arguments                                       │ │
│  │  • Too complex functions                                    │ │
│  │  • Duplicate code                                           │ │
│  └────────────────────────────────────────────────────────────┘ │
│                                                                   │
│  DURATION: ~10-15 seconds                                         │
│                                                                   │
│  BUILD STATUS:                                                    │
│  • No issues found → SUCCESS ✅                                  │
│  • Issues found → UNSTABLE ⚠️ (Pipeline continues)               │
│  • Critical errors → FAILED ❌ (Rare)                            │
│                                                                   │
│  WHY LINT?                                                        │
│  ✅ Catch bugs early                                             │
│  ✅ Enforce coding standards                                     │
│  ✅ Improve code readability                                     │
│  ✅ Reduce technical debt                                        │
│  ✅ Team consistency                                             │
│                                                                   │
│  SUCCESS CRITERIA:                                                │
│  ✅ Pylint executed successfully                                 │
│  ✅ Results captured                                             │
│  ✅ Report generated                                             │
│                                                                   │
│  NOTE:                                                            │
│  Pipeline continues even with lint violations to provide         │
│  visibility without blocking development                         │
│                                                                   │
└──────────────────────────────────────────────────────────────────┘
```

---

### Speaker Notes:
"Stage 5 runs pylint to check code quality. It looks for style violations, potential bugs, unused variables, and more. Important: if issues are found, the build is marked UNSTABLE but continues. We want visibility into code quality without blocking the pipeline. This gives developers feedback while keeping the workflow moving."
