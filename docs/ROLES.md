# Team Roles

airDJ is a pipeline: each step takes the previous step's output, does one job, and passes the result on. **Each of us owns part of that pipeline, including any screens related to it.** There is no backend; everything runs on the phone.

## How the pieces connect

```
 User swipes right
        │
        ▼
 ① Camera + Hand Tracking   → HandFrame      (raw hand position)        App & Vision        [Mac]
        │
        ▼
 ② Smoothing                → HandFrame      (jitter removed)           Dispatch & Quality  [Windows OK]
        │
        ▼
 ③ Gesture Detection        → GestureEvent   (.swipeRight)              Gesture Detection   [Windows OK]
        │
        ▼
 ④ Dispatch                 → MusicAction    (.next)                    Dispatch & Quality  [Windows OK]
        │
        ▼
 ⑤ Music Control            → song skips in Spotify / Apple Music       Lead + Integrations [Mac]
```

`HandFrame`, `GestureEvent`, and `MusicAction` are the **handoffs** between steps. The lead defines them in Week 1 and the team reviews them. After that, as long as everyone sends and receives those exact shapes, nobody needs to know how anyone else's code works inside.

## Mac vs. Windows

Building and running the iOS app requires **Xcode, which only runs on a Mac**. So the code is split into two parts:

| Part | What's in it | Who works on it | Runs on |
|---|---|---|---|
| **iOS app** | Camera, Vision, Spotify, Apple Music, all screens | The two Mac owners | Mac + iPhone |
| **`AirDJCore` Swift package** | Handoff types, smoothing, gesture detection, dispatch, tests | Everyone, but owned by the two Windows teammates | Mac **or Windows** (`swift test`) |

`AirDJCore` uses **no Apple frameworks**: just numbers in, decisions out. That's what lets the Windows teammates build and test their work fully on their own machines. CI runs everything on a cloud Mac for every pull request.

---

## Tech Lead + Integrations (Mac): "the coordinator & the hands"

**Builds:** Apple Music and Spotify control behind one shared `MusicController` protocol, so the rest of the app doesn't care which service is active. Also builds the **Now Playing** screen (album art, title, artist) and handles Spotify login, dropped connections, and error messages.

**Runs:** defines the handoff types, reviews code, and leads integration at the end of each milestone.

**Week 1:** write the handoff types, create the GitHub board and first issues, and build a quick **Spotify proof of concept** (connect and skip one song).

> **Spotify fallback rule:** if Spotify isn't working reliably by the end of Milestone 1, we ship Apple Music first and move Spotify to Milestone 3. Spotify is the riskiest part of the project, so we decide this early rather than letting it block everything.

**Workload:** 🟠 Heavy (expected for the lead).

---

## App & Computer Vision (Mac): "the eyes & the app"

**Builds:**
- The front-camera pipeline (AVFoundation) and hand tracking (Vision), which outputs a `HandFrame` about 15 times a second. Makes sure "right" always means *the user's* right, since the front camera is mirrored.
- The **recording tool**, which saves hand movements to JSON files. ⚠️ **Top priority right after hand tracking works**, because both Windows teammates depend on these recordings.
- A debug overlay that draws dots on the hand.
- The app shell (the Xcode project and wiring all the pieces together), plus the **Onboarding** and **Settings** screens (needed in Milestone 3).
- Battery and performance tuning.

**Stretch:** train a custom gesture model with Create ML.

**Week 1:** create the Xcode project, get the camera on screen, and draw the hand dots.

**Workload:** 🟠 Heavy. The work is wide rather than deep, and the screens come later, so it's spread out over time.

---

## Gesture Detection (Windows): "the brain"

**Builds:** the logic in `AirDJCore` that watches `HandFrame`s over time and decides when a gesture happened:
- **Swipe left/right:** the hand moves far and fast horizontally.
- **Palm hold:** an open hand stays still for about 1 second. It must *not* fire during a swipe.
- Sensitivity settings (low / medium / high) that the Settings screen can adjust.

**Stretch:** thumbs-up gesture.

**Week 1:** write a recognizer skeleton and a first test using hand-written fake data. No camera needed.

**Workload:** 🟡 Medium. The scope is narrow, but getting gestures to work reliably takes a lot of tuning.

---

## Dispatch & Quality (Windows): "the filter & the judge"

**Builds:**
- **Smoothing:** raw hand positions from Vision jitter frame to frame, which causes false gestures. Build a smoothing filter (look up the **One Euro filter**) that cleans up `HandFrame`s before they reach Gesture Detection. Measure the improvement.
- **Dispatch:** turns gestures into music actions, with a cooldown so one swipe never skips three songs.
- **Test framework:** loads recorded JSON files and replays them through the pipeline.
- **Accuracy report:** runs every recording and reports accuracy and false triggers. The README's metrics come from this.
- **Recording visualizer:** a small tool that plots recorded hand movement, so the team can *see* what a swipe looks like when tuning.
- **CI:** the GitHub Actions setup that builds and tests every pull request.

**Week 1:** create the `AirDJCore` package and set up CI so `swift test` runs on every pull request.

**Workload:** 🟡 Medium.

---

## Working in parallel

Nobody waits on anyone. Until the real piece before yours exists, use a stand-in:

| Role | Stand-in |
|---|---|
| App & Vision | None needed; you're first |
| Gesture Detection | Hand-written fake data, then real recordings |
| Dispatch & Quality | Hand-written fake data and fake gestures, then real recordings |
| Lead + Integrations | Debug Next / Previous / Pause buttons |

**Recordings:** early in Milestone 2, we record a big batch of gestures from all 4 of us (different lighting and distances, plus "random movement" that should *not* trigger anything). That gives the Windows teammates plenty of real data to work with.

**Integration:** at the end of each milestone, we swap the stand-ins for the real pieces and test the whole app on a real iPhone.

## Ground rules

- Every change gets an issue, a branch, and a pull request. **Never push directly to `main`.**
- At least one teammate reviews each PR.
- Test camera, gesture, and music features on a real iPhone before calling them done.
- Stuck for more than a day? Tell the lead.
