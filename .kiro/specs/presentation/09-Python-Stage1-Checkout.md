# Slide 9: Python Pipeline - Stage 1: Checkout

## Stage 1: Checkout Code from Repository

### Visual: Detailed Stage Breakdown

```
┌──────────────────────────────────────────────────────────────────┐
│                    STAGE 1: CHECKOUT                              │
├──────────────────────────────────────────────────────────────────┤
│                                                                   │
│  PURPOSE:                                                         │
│  Clone source code from Git repository to Jenkins workspace      │
│                                                                   │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │                                                             │ │
│  │   GitHub Repository                                         │ │
│  │   ┌─────────────────────────────────────────┐             │ │
│  │   │  Python-Testing/                        │             │ │
│  │   │  ├── src/                               │             │ │
│  │   │  │   └── calculator/                    │             │ │
│  │   │  ├── tests/                             │             │ │
│  │   │  ├── requirements.txt                   │             │ │
│  │   │  ├── ci-config.yaml                     │             │ │
│  │   │  └── Jenkinsfile                        │             │ │
│  │   └─────────────────────────────────────────┘             │ │
│  │                      │                                      │ │
│  │                      │ git clone                           │ │
│  │                      ▼                                      │ │
│  │   Jenkins Workspace                                         │ │
│  │   ┌─────────────────────────────────────────┐             │ │
│  │   │  /workspace/Python-Testing/             │             │ │
│  │   │  ├── src/                               │             │ │
│  │   │  ├── tests/                             │             │ │
│  │   │  ├── requirements.txt                   │             │ │
│  │   │  ├── ci-config.yaml                     │             │ │
│  │   │  └── Jenkinsfile                        │             │ │
│  │   └─────────────────────────────────────────┘             │ │
│  │                                                             │ │
│  └────────────────────────────────────────────────────────────┘ │
│                                                                   │
│  TOOLS USED:                                                      │
│  • Git                                                            │
│  • Jenkins SCM Plugin                                             │
│                                                                   │
│  CONFIGURATION:                                                   │
│  • Repository URL: From Jenkinsfile or project config            │
│  • Branch: main/master (configurable)                            │
│  • Credentials: Jenkins credentials store                        │
│  • Timeout: 5 minutes                                             │
│                                                                   │
│  CODE EXECUTED:                                                   │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  def checkout(String repoURL, String branch, Map config) { │ │
│  │      checkout scm  // Uses default SCM configuration        │ │
│  │      // OR                                                  │ │
│  │      checkout([                                             │ │
│  │          $class: 'GitSCM',                                  │ │
│  │          branches: [[name: branch]],                        │ │
│  │          userRemoteConfigs: [[url: repoURL]]                │ │
│  │      ])                                                     │ │
│  │  }                                                          │ │
│  └────────────────────────────────────────────────────────────┘ │
│                                                                   │
│  DURATION: ~5 seconds                                             │
│                                                                   │
│  SUCCESS CRITERIA:                                                │
│  ✅ Code successfully cloned                                     │
│  ✅ All files present in workspace                               │
│  ✅ No Git errors                                                 │
│                                                                   │
│  POSSIBLE FAILURES:                                               │
│  ❌ Repository not accessible                                    │
│  ❌ Invalid credentials                                          │
│  ❌ Network issues                                               │
│  ❌ Branch doesn't exist                                         │
│                                                                   │
└──────────────────────────────────────────────────────────────────┘
```

---

### Speaker Notes:
"Stage 1 is straightforward - we clone the code from Git. The library uses Jenkins' built-in SCM functionality. It takes about 5 seconds and handles authentication automatically using Jenkins credentials."
