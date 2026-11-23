# [App Name]

**Control Spotify and Apple Music with hand gestures on your iPhone.**
Prop up your phone, wave your hand, and skip songs without touching anything. Handy when you're cooking, working out, or your hands are full.

> 🚧 **Status:** Early development. We're a team of 4 students building this as a learning project.

---

## What it does

The app uses your iPhone's front camera to watch for your hand. When you make a gesture, it controls the music you're listening to.

| Gesture | Action |
|---|---|
| 👉 Swipe right | Next song |
| 👈 Swipe left | Previous song |
| ✋ Open palm | Pause / play |

Supported music apps: **Spotify** and **Apple Music**.

---

## Features

### MVP (what we're building first)
- [ ] Show the camera on screen
- [ ] Detect a hand using Apple's Vision framework
- [ ] Detect left and right swipes
- [ ] Skip songs in Spotify
- [ ] Skip songs in Apple Music
- [ ] Pause / play with an open palm
- [ ] A clean main screen showing what's playing

### Bonus (if we have time)
- [ ] 👍 Thumbs up to like a song
- [ ] Train a custom gesture model with Create ML
- [ ] Settings screen (sensitivity, which gestures are on)
- [ ] Release a beta on TestFlight so friends can try it

---

## How it works

1. **Camera:** the front camera captures video.
2. **Hand tracking:** Apple's Vision framework finds points on your hand (fingertips, wrist, etc.) in each frame.
3. **Gesture detection:** we track how your hand moves. If it moves far enough left or right quickly, that counts as a swipe.
4. **Music control:** the app tells Spotify or Apple Music to skip, go back, or pause.

Everything runs on the phone. No video is saved or sent anywhere.

---

## Tech stack

- **Swift** and **SwiftUI**: the app and its screens
- **AVFoundation**: camera access
- **Vision**: hand tracking
- **Spotify iOS SDK**: controlling Spotify
- **MediaPlayer**: controlling Apple Music
- **Create ML / Core ML**: custom gestures (bonus)

---

## Getting started

### What you need
- A **Mac** with **Xcode** installed (free on the Mac App Store)
- An **iPhone** running iOS 17 or later (the simulator has no camera, so you need a real phone)
- The **Spotify** app and/or **Apple Music** on your iPhone

### Running the app
> Setup instructions coming soon. The Xcode project hasn't been created yet.

---

## Project structure (planned)

```
Camera/         Camera capture
HandTracking/   Finding the hand with Vision
Gestures/       Turning hand movement into swipes and gestures
Music/          Spotify and Apple Music controls
UI/             Screens and buttons
```

---

## How we work

1. Pick an issue from the **project board** and assign it to yourself.
2. Make a new branch for it (example: `swipe-detection`).
3. When it works, open a **pull request**.
4. A teammate looks it over, then it gets merged into `main`.

Don't push straight to `main`!

---

## Team

| Name | Role |
|---|---|
| [Name] | Project lead |
| [Name] | Camera and hand tracking |
| [Name] | Gestures |
| [Name] | Music integration and UI |
