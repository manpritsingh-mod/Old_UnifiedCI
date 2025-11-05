# Slide 13: Python Pipeline - Stage 7: Test Execution (Parallel)

## Stage 7: Comprehensive Testing with Parallel Execution

### Visual: Parallel Test Execution

```
┌──────────────────────────────────────────────────────────────────┐
│            STAGE 7: TEST EXECUTION (PARALLEL)                     │
├──────────────────────────────────────────────────────────────────┤
│                                                                   │
│  PURPOSE:                                                         │
│  Run comprehensive test suite with parallel execution for speed  │
│                                                                   │
│  PARALLEL EXECUTION:                                              │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │                                                             │ │
│  │  ┌──────────────────────┐    ┌──────────────────────────┐ │ │
│  │  │   UNIT TESTS         │    │   FUNCTIONAL TESTS       │ │ │
│  │  │   (Left Branch)      │    │   (Right Branch)         │ │ │
│  │  │                      │    │                          │ │ │
│  │  │  Tool: pytest        │    │  Tool: pytest            │ │ │
│  │  │  Coverage: Yes       │    │  Coverage: No            │ │ │
│  │  │                      │    │                          │ │ │
│  │  │  Test Files:         │    │  Sub-Stages:             │ │ │
│  │  │  • test_calculator   │    │  ┌────────────────────┐ │ │ │
│  │  │    .py               │    │  │ 1. Smoke Tests     │ │ │ │
│  │  │  • test_app.py       │    │  │    • Basic checks  │ │ │ │
│  │  │                      │    │  │    • Imports       │ │ │ │
│  │  │  Test Count: 50+     │    │  │    • Environment   │ │ │ │
│  │  │                      │    │  └────────────────────┘ │ │ │
│  │  │  Tests:              │    │  ┌────────────────────┐ │ │ │
│  │  │  • test_addition     │    │  │ 2. Sanity Tests    │ │ │ │
│  │  │  • test_subtraction  │    │  │    • Core workflow │ │ │ │
│  │  │  • test_multiply     │    │  │    • Integration   │ │ │ │
│  │  │  • test_divide       │    │  │    • Config load   │ │ │ │
│  │  │  • test_power        │    │  └────────────────────┘ │ │ │
│  │  │  • test_sqrt         │    │  ┌────────────────────┐ │ │ │
│  │  │  • test_factorial    │    │  │ 3. Regression      │ │ │ │
│  │  │  • test_even_odd     │    │  │    Tests           │ │ │ │
│  │  │  • test_circle_area  │    │  │    • Bug fixes     │ │ │ │
│  │  │  • test_exceptions   │    │  │    • Edge cases    │ │ │ │
│  │  │  • ...               │    │  │    • Memory leaks  │ │ │ │
│  │  │                      │    │  └────────────────────┘ │ │ │
│  │  │  Duration: ~15s      │    │  Duration: ~20s        │ │ │
│  │  │                      │    │                          │ │ │
│  │  └──────────────────────┘    └──────────────────────────┘ │ │
│  │                                                             │ │
│  │  Total Duration: ~20 seconds (parallel, not 35 seconds!)   │ │
│  │                                                             │ │
│  └────────────────────────────────────────────────────────────┘ │
│                                                                   │
│  UNIT TESTS DETAILS:                                              │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  Command:                                                   │ │
│  │  $ venv/bin/pytest --verbose --tb=short                     │ │
│  │    --cov=src/calculator --cov-report=html                   │ │
│  │    --cov-report=xml --junit-xml=test-results.xml            │ │
│  │                                                             │ │
│  │  Output:                                                    │ │
│  │  ============================= test session starts ======== │ │
│  │  platform linux -- Python 3.11.0                            │ │
│  │  plugins: cov-4.1.0                                         │ │
│  │  collected 52 items                                         │ │
│  │                                                             │ │
│  │  tests/test_calculator.py::test_addition PASSED      [ 1%] │ │
│  │  tests/test_calculator.py::test_subtraction PASSED   [ 3%] │ │
│  │  tests/test_calculator.py::test_multiply PASSED      [ 5%] │ │
│  │  tests/test_calculator.py::test_divide PASSED        [ 7%] │ │
│  │  ...                                                        │ │
│  │  tests/test_app.py::test_binary_operation PASSED     [98%] │ │
│  │  tests/test_app.py::test_process_operation PASSED   [100%] │ │
│  │                                                             │ │
│  │  ============================= 52 passed in 2.34s ========= │ │
│  │                                                             │ │
│  │  Coverage Report:                                           │ │
│  │  Name                    Stmts   Miss  Cover                │ │
│  │  ─────────────────────────────────────────────             │ │
│  │  src/calculator.py         45      2    96%                │ │
│  │  src/app.py                67      5    93%                │ │
│  │  ─────────────────────────────────────────────             │ │
│  │  TOTAL                    112      7    94%                │ │
│  └────────────────────────────────────────────────────────────┘ │
│                                                                   │
│  FUNCTIONAL TESTS DETAILS:                                        │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  Smoke Tests:                                               │ │
│  │  $ venv/bin/pytest tests/smoke/ -v --tb=short               │ │
│  │  • test_application_imports                                 │ │
│  │  • test_basic_functionality                                 │ │
│  │  • test_environment_setup                                   │ │
│  │  Result: 5 passed                                           │ │
│  │                                                             │ │
│  │  Sanity Tests:                                              │ │
│  │  $ venv/bin/pytest tests/sanity/ -v --tb=short              │ │
│  │  • test_core_workflow                                       │ │
│  │  • test_configuration_loading                               │ │
│  │  • test_dependency_availability                             │ │
│  │  Result: 6 passed                                           │ │
│  │                                                             │ │
│  │  Regression Tests:                                          │ │
│  │  $ venv/bin/pytest tests/regression/ -v --tb=short          │ │
│  │  • test_bug_fix_001                                         │ │
│  │  • test_edge_case_handling                                  │ │
│  │  • test_memory_leak_fix                                     │ │
│  │  Result: 8 passed                                           │ │
│  └────────────────────────────────────────────────────────────┘ │
│                                                                   │
│  CODE EXECUTED:                                                   │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  parallel {                                                 │ │
│  │      // Branch 1: Unit Tests                                │ │
│  │      'Unit Test': {                                         │ │
│  │          def testResult = core_test.runUnitTest(config)     │ │
│  │          env.UNIT_TEST_RESULT = testResult                  │ │
│  │      }                                                      │ │
│  │                                                             │ │
│  │      // Branch 2: Functional Tests                          │ │
│  │      'Functional Tests': {                                  │ │
│  │          stage('Smoke Tests') {                             │ │
│  │              sh PythonScript.venvSmokeTestLinuxCommand()    │ │
│  │          }                                                  │ │
│  │          stage('Sanity Tests') {                            │ │
│  │              sh PythonScript.venvSanityTestLinuxCommand()   │ │
│  │          }                                                  │ │
│  │          stage('Regression Tests') {                        │ │
│  │              sh PythonScript.venvRegressionTestLinuxCommand()│ │
│  │          }                                                  │ │
│  │      }                                                      │ │
│  │  }                                                          │ │
│  └────────────────────────────────────────────────────────────┘ │
│                                                                   │
│  ARTIFACTS GENERATED:                                             │
│  • test-results.xml (JUnit format)                               │
│  • coverage.xml (Coverage data)                                  │
│  • htmlcov/ (HTML coverage report)                               │
│  • .coverage (Coverage database)                                 │
│                                                                   │
│  DURATION: ~20 seconds (parallel execution)                       │
│  (Would be ~35 seconds if sequential)                             │
│                                                                   │
│  BUILD STATUS:                                                    │
│  • All tests pass → SUCCESS ✅                                   │
│  • Some tests fail → UNSTABLE ⚠️                                 │
│  • Critical failure → UNSTABLE ⚠️                                │
│                                                                   │
│  WHY PARALLEL EXECUTION?                                          │
│  ✅ Faster feedback (40% time savings)                           │
│  ✅ Better resource utilization                                  │
│  ✅ Scales with test suite size                                  │
│                                                                   │
│  SUCCESS CRITERIA:                                                │
│  ✅ All test suites executed                                     │
│  ✅ Test results captured                                        │
│  ✅ Coverage data generated                                      │
│  ✅ No critical errors                                           │
│                                                                   │
└──────────────────────────────────────────────────────────────────┘
```

---

### Speaker Notes:
"Stage 7 is where the magic happens - parallel test execution. Unit tests run on one branch with coverage, while functional tests (smoke, sanity, regression) run on another. This parallel approach saves 40% of time. We run 50+ unit tests plus functional tests, all completing in about 20 seconds. If tests fail, the build is marked UNSTABLE but continues to generate reports."
