# NIGHTFALL

![Nightfall Logo](Assets/Graphics/UI/Logo/nightfall-logo.png)

## Overview

Nightfall is a first-person psychological horror game that follows a team of paranormal investigators trapped in the cursed town of Ravenbrook. As nightfall brings deadly creatures and supernatural phenomena, players must uncover the town's dark history and find a way to survive the endless night.

**Engine:** Unity 6  
**Platform:** PC  
**Target Release:** Q4 2026  
**Development Status:** Pre-production  
**Studio:** New Day Studios

**Repository:** https://github.com/New-Day-Studios/nightfall-game

## Table of Contents

- [Project Structure](#project-structure)
- [Setup & Installation](#setup--installation)
- [Branching Strategy](#branching-strategy)
- [Coding Standards](#coding-standards)
- [Asset Pipeline](#asset-pipeline)
- [Build Process](#build-process)
- [Performance Requirements](#performance-requirements)
- [Documentation](#documentation)
- [Testing](#testing)
- [Communication](#communication)
- [License & Legal](#license--legal)

## Project Structure

```
nightfall-game/
├── Assets/
│   ├── Animations/          # Animation clips and controllers
│   ├── Audio/               # Sound effects, music, and ambient sounds
│   │   ├── Ambient/         # Environmental and background sounds
│   │   ├── Creatures/       # Enemy and creature sounds
│   │   ├── Dialogue/        # Character voice acting
│   │   ├── Music/           # Game soundtrack
│   │   └── SFX/             # Sound effects
│   ├── Graphics/
│   │   ├── Materials/       # Materials and shaders
│   │   ├── Models/          # 3D models
│   │   ├── Textures/        # Texture maps
│   │   ├── UI/              # Interface graphics
│   │   └── VFX/             # Visual effects
│   ├── Prefabs/             # Reusable game objects
│   │   ├── Characters/      # Player and NPC prefabs
│   │   ├── Creatures/       # Enemy prefabs
│   │   ├── Environment/     # Environmental prefabs
│   │   ├── Interactables/   # Interactive object prefabs
│   │   └── UI/              # Interface prefabs
│   ├── Resources/           # Assets loaded at runtime
│   ├── Scenes/              # Game levels and test scenes
│   │   ├── Levels/          # Main game levels
│   │   ├── Testing/         # Test environments
│   │   └── UI/              # Menu scenes
│   ├── Scripts/             # C# source code
│   │   ├── AI/              # Enemy behaviors and NPC systems
│   │   ├── Core/            # Core game systems
│   │   ├── Characters/      # Player and NPC controllers
│   │   ├── Gameplay/        # Game mechanics
│   │   ├── Interaction/     # Interactive elements
│   │   ├── Inventory/       # Inventory system
│   │   ├── UI/              # User interface scripts
│   │   ├── Save/            # Save system
│   │   ├── Audio/           # Audio management
│   │   └── Utils/           # Utility functions and helpers
│   └── Settings/            # Project settings and configurations
├── Packages/                # Package dependencies
├── ProjectSettings/         # Unity project settings
├── UserSettings/            # User-specific settings
└── Documentation/           # Development documentation
    ├── Design/              # Design documents
    ├── Technical/           # Technical documentation
    ├── Art/                 # Art guidelines and references
    └── Workflow/            # Workflow processes
```

## Setup & Installation

### System Requirements

**Developer Workstation:**
- Windows 10/11 64-bit
- 16GB RAM minimum (32GB recommended)
- Intel i5/AMD Ryzen 5 or better
- NVIDIA GTX 1060 Ti or better
- 200GB SSD free space

### Environment Setup

1. Install Unity Hub
2. Install Unity 6.0.x (specific version will be locked in ProjectSettings)
3. Install required dependencies:
   - Visual Studio 2022 (with Game Development with Unity workload)
   - Git LFS (for asset handling)
   - Node.js (for build automation)

### Repository Setup

```bash
# Clone the repository (requires Git LFS)
git clone https://github.com/New-Day-Studios/nightfall-game.git

# Initialize Git LFS
cd nightfall-game
git lfs install
git lfs pull

# Open project in Unity Hub
```

### Required Unity Packages

- Universal Render Pipeline (URP)
- Input System
- Cinemachine
- Post Processing
- TextMeshPro
- Addressables
- [Specific third-party assets TBD]

## Branching Strategy

We use a modified Git Flow strategy:

- `main` - Production-ready code
- `develop` - Integration branch for feature development
- `feature/*` - Individual feature branches
- `release/*` - Release preparation branches
- `hotfix/*` - Emergency fixes for production

All commits must reference a ticket number from JIRA.

### Workflow

1. Pull latest `develop` branch
2. Create a feature branch (`feature/NF-123-feature-name`)
3. Implement your changes
4. Submit a pull request to `develop`
5. Code review and approval required before merging
6. CI builds and tests must pass

## Coding Standards

### C# Style Guide

- Follow [Microsoft C# Coding Conventions](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions)
- Use `NF` prefix for all namespaces (e.g., `NewDay.Nightfall.Gameplay`)
- Class organization:
  1. Private fields
  2. Properties
  3. Unity message methods (Awake, Start, Update, etc.)
  4. Public methods
  5. Private methods

```csharp
// Example class structure
using UnityEngine;
using System.Collections;
using NewDay.Nightfall.Utils;

namespace NewDay.Nightfall.Gameplay
{
    public class NFExampleClass : MonoBehaviour
    {
        [SerializeField] private float _someValue;
        private bool _isActive;
        
        public bool IsActive => _isActive;
        
        private void Awake()
        {
            // Initialization
        }
        
        private void Start()
        {
            // Start setup
        }
        
        private void Update()
        {
            // Per-frame logic
        }
        
        public void DoSomething()
        {
            // Public method
        }
        
        private void InternalMethod()
        {
            // Private method
        }
    }
}
```

### Unity-Specific Guidelines

- Use `[SerializeField]` instead of public fields
- Use properties for public access
- Use coroutines for time-based operations
- Use ScriptableObjects for data-driven design
- Avoid FindObjectOfType and GameObject.Find in production code
- Implement object pooling for frequently instantiated objects
- Use UnityEvents for inspector-assignable callbacks

## Asset Pipeline

### Naming Conventions

- **Textures:** `T_[AssetName]_[Type]`
  - Example: `T_WoodFloor_Diffuse`, `T_WoodFloor_Normal`
- **Materials:** `M_[AssetName]`
  - Example: `M_WoodFloor`
- **Models:** `SM_[AssetName]` (Static), `SK_[AssetName]` (Skinned)
  - Example: `SM_Chair`, `SK_Monster_01`
- **Animations:** `A_[CharacterName]_[AnimationName]`
  - Example: `A_Emma_Walk`
- **Audio:** `S_[Category]_[AssetName]`
  - Example: `S_Ambient_CatacombDrips`
- **Prefabs:** `PF_[Category]_[AssetName]`
  - Example: `PF_Prop_Lantern`
- **Scripts:** `NF[ScriptName]`
  - Example: `NFPlayerController`

### Asset Creation Guidelines

- **Textures:**
  - PBR workflow (Albedo, Metallic, Smoothness, Normal)
  - Power of 2 texture dimensions (1024, 2048, 4096)
  - Proper compression settings based on usage
  - Texture atlasing for common elements
  
- **Models:**
  - Environment assets: LODs required for complex objects
  - Characters: 15-30K triangles for main characters
  - Proper UV layout with texture space optimization
  - All meshes must have appropriate colliders
  
- **Audio:**
  - 44.1kHz, 16-bit WAV format for source files
  - Compression based on sound type (speech vs. ambient)
  - Spatial audio configuration for 3D sounds

## Build Process

### Local Development Build

1. Set build platform to Windows (64-bit)
2. Set build configuration (Development, Debug, Release)
3. Build from Unity Editor (File > Build Settings)

### CI/CD Pipeline

We use GitHub Actions for automated builds:
- Pull request builds for verification
- Nightly builds from develop branch
- Release candidate builds from release branches
- Production builds from main branch

Build artifacts are stored in AWS S3 and linked to JIRA tickets.

## Performance Requirements

### Minimum System Requirements (Target Players)

- OS: Windows 10 64-bit
- CPU: Intel Core i5-6500 / AMD Ryzen 3 1300X
- GPU: NVIDIA GTX 1060 Ti 6GB
- RAM: 8GB
- Storage: 50GB available space
- DirectX: Version 11

### Target Performance Metrics

- Frame Rate: 60 FPS @ 1080p on recommended specs
- Load Times: Initial load <30 seconds, level transitions <10 seconds
- Memory Usage: <4GB RAM in normal gameplay
- Draw Calls: <2000 per frame target

### Optimization Guidelines

- Use occlusion culling for complex environments
- Implement dynamic LOD system
- Bake lighting where appropriate
- Object pooling for particles and instantiated objects
- Optimize shader complexity for mid-range GPUs
- Profile regularly with Unity Profiler

## Documentation

### Design Documentation

Design documents are stored in `/Documentation/Design/` and include:
- Game Design Document (GDD)
- Level Design Documents
- Narrative Bible
- Character Profiles
- Enemy Design Specs

### Technical Documentation

Technical documentation is stored in `/Documentation/Technical/` and includes:
- Architecture Overview
- System Design Documents
- API Documentation (generated with DocFX)
- Performance Guidelines

### Workflow Documentation

Workflow documentation is stored in `/Documentation/Workflow/` and includes:
- Asset Creation Pipelines
- Quality Assurance Processes
- Build Procedures

## Testing

### Unit Tests

- Use Unity Test Framework for unit testing
- Tests located in `Assets/Scripts/Tests/`
- CI runs tests on every PR

### Playtest Process

1. Weekly internal playtests
2. Playtest feedback tracked in JIRA
3. Monthly milestone builds for extended testing
4. Regular performance profiling sessions

## Communication

- JIRA: Task tracking and project management
- Discord: Daily communication and quick questions
- Google Drive: Document sharing and collaboration
- Weekly Meetings: Sprint planning and retrospectives
- Monthly Reviews: Progress reviews and milestone assessments

## License & Legal

- Code: Proprietary, Copyright © 2025 New Day Studios
- Third-party assets: See `/Documentation/Legal/ThirdPartyLicenses.md`
- Development builds are confidential and subject to NDA
- See EULA and Terms of Service for complete legal terms

---

© 2025 New Day Studios. All rights reserved.
