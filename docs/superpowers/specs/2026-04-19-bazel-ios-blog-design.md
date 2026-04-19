# Blog Design: Bazel Integration in iOS

**Date:** 2026-04-19  
**Platform:** Hashnode  
**Format:** Story-driven tutorial (personal experience)  
**Target read time:** 8-10 min (adaptive depth)

---

## Audience

iOS developers who know Xcode/Swift but are new to Bazel. Assumes no prior build system knowledge beyond Xcode.

## Goals

1. Reader walks away with a working "Hello World" iOS app built with Bazel
2. Reader can evaluate whether to adopt Bazel in their own project

---

## Blog Structure

### Section 1 — Hook & Why Bazel *(medium detail)*
- Opens with relatable pain: slow Xcode builds, no caching, CI taking forever
- Introduces Bazel as what Google/Uber/Airbnb use internally
- Key advantages:
  - Hermetic, reproducible builds
  - Remote & local caching (only rebuilds what changed)
  - Language-agnostic, scales to monorepos
  - Works with Xcode via `rules_xcodeproj`
- Fun tone throughout

### Section 2 — Prerequisites & Project Structure *(short)*
- Install checklist: Bazel via Homebrew, Xcode, basic Swift knowledge
- Shows final file tree:
  ```
  MyApp/
  ├── MODULE.bazel
  ├── BUILD.bazel
  ├── .bazelrc
  ├── Sources/
  │   └── BazelApp.swift
  └── Resources/
      ├── BUILD.bazel
      └── Info.plist
  ```
- Shows `Sources/BazelApp.swift` — the actual SwiftUI entry point (`@main`, `WindowGroup`, `Text("Hello, world!")`)
- Fun line: "No Podfile, no Package.swift, no Cartfile. Just BUILD files."

### Section 3 — MODULE.bazel *(detailed)*
- Explains role: Bazel's dependency manager (like Package.swift for the build system)
- Line-by-line walkthrough of actual file:
  - `rules_apple` → provides `ios_application`
  - `rules_swift` → provides `swift_library`
  - `rules_xcodeproj` → generates Xcode project
- Fun line comparing to Package.swift fights with Xcode

### Section 4 — BUILD.bazel + Dependency Graph *(detailed)*
- Line-by-line walkthrough:
  - `swift_library` — compiles Swift sources
  - `ios_application` — app target, `deps` explained with `:` prefix syntax
  - `xcodeproj` — generates `.xcodeproj`
- **Mermaid diagram**: dependency graph showing target relationships
  ```
  NewApp → Source (swift_library)
  NewApp → Info.plist
  xcodeproj → NewApp
  ```
- Fun line about `project.pbxproj` crying

### Section 5 — .bazelrc *(short)*
- One line explanation: `--enable_bzlmod` enables modern dependency system
- Fun comparison to long Podfiles

### Section 6 — How Bazel Caching Works *(medium, new mini-section)*
- Explains hermetic builds and content hashing
- **Mermaid flowchart**: cache hit/miss flow
  ```
  Code change → Hash inputs → Cache hit? → Skip rebuild
                             → Cache miss? → Rebuild → Store in cache
  ```
- Explains WHY this is faster than Xcode's incremental builds

### Section 7 — Running Your First Build *(detailed)*
- Step-by-step commands with expected output:
  - `bazel run //:xcodeproj` — generate Xcode project
  - `bazel build //:NewApp` — build the app
  - `open Ganesha.xcodeproj` — open in Xcode
- Deep dive on `//` and `:` target syntax (common beginner confusion)
- Fun line about first successful build feeling like wizardry

### Section 8 — Should YOU Adopt Bazel? *(medium)*
- Honest two-column evaluation:
  - Use Bazel if: 3+ devs, slow CI, monorepo plans, reproducibility needed
  - Stick with Xcode if: solo dev, small app, deadline pressure
- Fun standing desk analogy

### Section 9 — Closing & What's Next *(short)*
- Recap what reader built
- Tease next steps: third-party deps, multi-module, remote caching with CI
- Link to GitHub repo
- Personal sign-off

---

## Tone

- Conversational, first-person ("I built this over a weekend")
- Light humor at natural transition points — relatable dev frustration, not forced
- No emoji spam
- Code blocks for every file/command

## Technical Accuracy

- All code examples sourced from actual project files
- Bazel versions: rules_apple 3.15.0, rules_swift 2.2.0, rules_xcodeproj 2.9.2
- Bzlmod enabled via `.bazelrc`
