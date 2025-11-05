# Slide 6: Architecture Overview

## Shared Library Structure (High-Level)

### Visual: Your Architecture Diagram

```
┌──────────────────────────────────────────────────────────────────┐
│              UNIFIED CI SHARED LIBRARY ARCHITECTURE               │
├──────────────────────────────────────────────────────────────────┤
│                                                                   │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │                  SHARED LIBRARY REPOSITORY                  │ │
│  │                                                             │ │
│  │  ┌──────────────────┐      ┌──────────────────┐          │ │
│  │  │  vars/ (Core)    │      │  src/ (Classes)  │          │ │
│  │  │                  │      │                  │          │ │
│  │  │  • Templates     │      │  • Script        │          │ │
│  │  │  • Core Utils    │      │    Generators    │          │ │
│  │  │  • Builders      │      │  • Docker Mgr    │          │ │
│  │  │  • Testers       │      │  • Git Mgr       │          │ │
│  │  └──────────────────┘      └──────────────────┘          │ │
│  │                                                             │ │
│  │  ┌──────────────────┐      ┌──────────────────┐          │ │
│  │  │  Core Logic      │      │  Pipeline        │          │ │
│  │  │                  │      │  Templates       │          │ │
│  │  │  • Build         │      │                  │          │ │
│  │  │  • Test          │      │  • Python        │          │ │
│  │  │  • Lint          │      │  • Java Maven    │          │ │
│  │  │  • Report        │      │  • Java Gradle   │          │ │
│  │  │  • Notify        │      │  • React         │          │ │
│  │  └──────────────────┘      └──────────────────┘          │ │
│  │                                                             │ │
│  │  ┌──────────────────┐      ┌──────────────────┐          │ │
│  │  │  Command Gen     │      │  Core Utilities  │          │ │
│  │  │                  │      │                  │          │ │
│  │  │  • Maven         │      │  • Config Reader │          │ │
│  │  │  • Gradle        │      │  • Language      │          │ │
│  │  │  • Python        │      │    Detection     │          │ │
│  │  │  • React         │      │  • Logger        │          │ │
│  │  └──────────────────┘      └──────────────────┘          │ │
│  │                                                             │ │
│  └────────────────────────────────────────────────────────────┘ │
│                                                                   │
│                    CONSUMED BY PROJECT TEAMS                      │
│                                                                   │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐      │
│  │  Team A      │    │  Team B      │    │  Team C      │      │
│  │  (Python)    │    │  (Java)      │    │  (React)     │      │
│  │              │    │              │    │              │      │
│  │  Jenkinsfile │    │  Jenkinsfile │    │  Jenkinsfile │      │
│  │  ci-config   │    │  ci-config   │    │  ci-config   │      │
│  └──────────────┘    └──────────────┘    └──────────────┘      │
│                                                                   │
└──────────────────────────────────────────────────────────────────┘
```

### Key Components:
1. **vars/** - Pipeline templates and core functions
2. **src/** - Reusable Groovy classes
3. **Templates** - Language-specific pipelines
4. **Utilities** - Common helper functions

---

### Speaker Notes:
"The library is organized into two main directories: 'vars' contains our pipeline templates and core functions, while 'src' has reusable classes. Teams simply import this library and use the appropriate template for their tech stack."
