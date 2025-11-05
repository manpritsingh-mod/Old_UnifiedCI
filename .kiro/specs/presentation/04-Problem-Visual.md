# Slide 4: Problem Visualization

## Before: The Chaos

### Visual: Multiple Projects with Different Pipelines

```
┌──────────────────────────────────────────────────────────────────┐
│                    BEFORE SHARED LIBRARY                          │
├──────────────────────────────────────────────────────────────────┤
│                                                                   │
│  Project A (Python)          Project B (Python)                  │
│  ┌─────────────────┐        ┌─────────────────┐                 │
│  │ Jenkinsfile     │        │ Jenkinsfile     │                 │
│  │ (250 lines)     │        │ (180 lines)     │                 │
│  │                 │        │                 │                 │
│  │ • Custom stages │        │ • Different     │                 │
│  │ • pytest        │        │   stages        │                 │
│  │ • No linting    │        │ • unittest      │                 │
│  │ • Manual Docker │        │ • pylint        │                 │
│  └─────────────────┘        │ • No Docker     │                 │
│                              └─────────────────┘                 │
│                                                                   │
│  Project C (Java)            Project D (Java)                    │
│  ┌─────────────────┐        ┌─────────────────┐                 │
│  │ Jenkinsfile     │        │ Jenkinsfile     │                 │
│  │ (300 lines)     │        │ (220 lines)     │                 │
│  │                 │        │                 │                 │
│  │ • Maven         │        │ • Gradle        │                 │
│  │ • JUnit         │        │ • TestNG        │                 │
│  │ • Checkstyle    │        │ • SpotBugs      │                 │
│  │ • Custom Docker │        │ • Different     │                 │
│  └─────────────────┘        │   Docker        │                 │
│                              └─────────────────┘                 │
│                                                                   │
│  ❌ No Consistency    ❌ Hard to Maintain    ❌ Time Wasted      │
│                                                                   │
└──────────────────────────────────────────────────────────────────┘
```

### Key Statistics:
- **Average Setup Time:** 2-3 days per project
- **Code Duplication:** 80% similar code across projects
- **Maintenance Effort:** 4-6 hours per update × 50 projects = 200-300 hours
- **Onboarding Time:** 1-2 weeks for new developers

---

### Speaker Notes:
"Here's what it looks like in reality. Four Python projects, each with completely different pipeline configurations. Some use pytest, others unittest. Some have linting, others don't. When we need to add a new feature or fix a bug, we have to update each one individually. This is the chaos we needed to solve."
