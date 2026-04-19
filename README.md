# bazel-ios-starter

A minimal iOS SwiftUI app built with Bazel. Companion project for the blog post: [Tired of Slow Xcode Builds? Meet Bazel](https://hashnode.com/@Hitarthbhatt).

## Prerequisites

```bash
# Install Bazel
brew install bazel

# Verify
bazel --version
```

Xcode must also be installed with at least one simulator.

## Setup

```bash
git clone https://github.com/Hitarthbhatt/bazel-ios-starter.git
cd bazel-ios-starter
```

## Run

**Generate the Xcode project:**
```bash
bazel run //:xcodeproj
```

**Build the app:**
```bash
bazel build //:NewApp
```

**Run directly in simulator (no Xcode needed):**
```bash
bazel run //:NewApp
```

**Or open in Xcode:**
```bash
open Ganesha.xcodeproj
```

Then select a simulator and hit `Cmd+R`.

## Project Structure

```
├── MODULE.bazel          # External dependencies (rules_apple, rules_swift, rules_xcodeproj)
├── BUILD.bazel           # Build targets (swift_library, ios_application, xcodeproj)
├── .bazelrc              # Enables Bzlmod
├── Sources/
│   └── BazelApp.swift    # SwiftUI entry point
└── Resources/
    ├── BUILD.bazel        # Exports Info.plist
    └── Info.plist
```

## Dependencies

| Rule | Version | Purpose |
|---|---|---|
| rules_apple | 4.1.1 | iOS build rules (Bazel 8 compatible) |
| rules_swift | 2.3.1 | Swift compilation |
| rules_xcodeproj | 2.10.0 | Xcode project generation |
