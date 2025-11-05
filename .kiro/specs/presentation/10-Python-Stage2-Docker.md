# Slide 10: Python Pipeline - Stage 2: Pull Docker Image

## Stage 2: Pull Docker Image from Nexus Registry

### Visual: Docker Image Management

```
┌──────────────────────────────────────────────────────────────────┐
│                 STAGE 2: PULL DOCKER IMAGE                        │
├──────────────────────────────────────────────────────────────────┤
│                                                                   │
│  PURPOSE:                                                         │
│  Pull Python Docker image to ensure consistent build environment │
│                                                                   │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │                                                             │ │
│  │   Nexus Docker Registry                                     │ │
│  │   (nexus.company.com:8082)                                  │ │
│  │                                                             │ │
│  │   ┌─────────────────────────────────────┐                 │ │
│  │   │  Available Python Images:           │                 │ │
│  │   │                                      │                 │ │
│  │   │  • python:3.8                        │                 │ │
│  │   │  • python:3.9                        │                 │ │
│  │   │  • python:3.11  ← (Default)          │                 │ │
│  │   │  • python:latest                     │                 │ │
│  │   │                                      │                 │ │
│  │   └─────────────────────────────────────┘                 │ │
│  │                      │                                      │ │
│  │                      │ docker pull                         │ │
│  │                      ▼                                      │ │
│  │   Jenkins Agent                                             │ │
│  │   ┌─────────────────────────────────────┐                 │ │
│  │   │  Docker Image Cache                 │                 │ │
│  │   │                                      │                 │ │
│  │   │  nexus.company.com:8082/            │                 │ │
│  │   │  dev/python:3.11                    │                 │ │
│  │   │                                      │                 │ │
│  │   │  Size: ~900 MB                      │                 │ │
│  │   │  Layers: 12                         │                 │ │
│  │   └─────────────────────────────────────┘                 │ │
│  │                                                             │ │
│  └────────────────────────────────────────────────────────────┘ │
│                                                                   │
│  TOOLS USED:                                                      │
│  • Docker Engine                                                  │
│  • Nexus Docker Registry                                          │
│  • DockerImageManager.groovy (Custom class)                      │
│                                                                   │
│  CONFIGURATION (from ci-config.yaml):                             │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  nexus:                                                     │ │
│  │    url: "https://nexus.company.com:8082"                    │ │
│  │    registry: "nexus.company.com:8082"                       │ │
│  │    credentials_id: "nexus-docker-creds"                     │ │
│  │    project: "dev"                                           │ │
│  │                                                             │ │
│  │  python_version: "3.11"  # Optional, defaults to 3.11      │ │
│  └────────────────────────────────────────────────────────────┘ │
│                                                                   │
│  CODE EXECUTED:                                                   │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │  def imageConfig = DockerImageManager.getImageConfig(      │ │
│  │      'python', config                                       │ │
│  │  )                                                          │ │
│  │  // Returns: nexus.company.com:8082/dev/python:3.11        │ │
│  │                                                             │ │
│  │  docker.withRegistry(imageConfig.registryUrl,               │ │
│  │                      imageConfig.credentialsId) {          │ │
│  │      def image = docker.image(imageConfig.imagePath)       │ │
│  │      image.pull()                                           │ │
│  │  }                                                          │ │
│  └────────────────────────────────────────────────────────────┘ │
│                                                                   │
│  DURATION:                                                        │
│  • First pull: ~15-30 seconds (downloads image)                  │
│  • Cached: ~5 seconds (uses local cache)                         │
│                                                                   │
│  WHY DOCKER?                                                      │
│  ✅ Consistent environment across all builds                     │
│  ✅ No "works on my machine" issues                              │
│  ✅ Isolated dependencies                                        │
│  ✅ Easy version management                                      │
│  ✅ Reproducible builds                                          │
│                                                                   │
│  SUCCESS CRITERIA:                                                │
│  ✅ Image pulled successfully                                    │
│  ✅ Image verified (docker images command)                       │
│  ✅ Python version matches expected                              │
│                                                                   │
│  POSSIBLE FAILURES:                                               │
│  ❌ Registry not accessible                                      │
│  ❌ Invalid credentials                                          │
│  ❌ Image not found in registry                                  │
│  ❌ Network timeout                                              │
│  ❌ Disk space full                                              │
│                                                                   │
└──────────────────────────────────────────────────────────────────┘
```

---

### Speaker Notes:
"Stage 2 pulls the Python Docker image from our Nexus registry. This is crucial - it ensures every build runs in the exact same environment. First pull takes 15-30 seconds, but subsequent builds use the cached image and complete in 5 seconds. This eliminates environment inconsistencies."
