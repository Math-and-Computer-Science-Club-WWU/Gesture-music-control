# 0001: Split core logic into the `AirDJCore` Swift package

- **Author:** Tech Lead
- **Date:** 2026-11-23
- **Status:** Accepted

## Context
iOS apps can only be built with Xcode, which runs only on macOS. Two of our four team members don't have Macs. We still want everyone to own real, substantial code that they can build and test on their own machines.

## Options considered
1. **Single Xcode project:** simplest setup, but the two Windows teammates couldn't build or test anything without borrowing a Mac.
2. **Cross-platform framework (Flutter / React Native):** still requires a Mac to build for iOS, and adds complexity on top of the camera and music integrations.
3. **Separate the platform-independent logic into a Swift package:** smoothing, gesture detection, and dispatch only work with numbers, so they don't need Apple frameworks.

## Decision
Option 3. Core logic lives in `AirDJCore`, a Swift package with **no dependency on Apple frameworks** (no Vision, AVFoundation, UIKit, or SwiftUI). The iOS app depends on it.

## Why
- Windows teammates can build and run tests with the official Swift toolchain (`swift test`).
- Core logic becomes independently testable with recorded data, without a camera or device.
- A clean boundary between "platform code" and "logic" is good architecture regardless.

## Consequences
- Shared types (`HandFrame`, `GestureEvent`, `MusicAction`) must be our own types, not Vision's.
- Contributors must not import Apple-only frameworks into `AirDJCore`; CI should build it on a non-Apple platform to enforce this.
- Windows teammates can only see their code running live on a phone during integration sessions with a Mac owner.
