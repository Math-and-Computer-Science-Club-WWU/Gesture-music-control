# Decision Records

This folder records important technical decisions: what we chose, what else we considered, and why.

**When to write one:** whenever you make a choice a teammate (or an interviewer) might ask "why did you do it that way?" Examples: choosing a smoothing algorithm, how palm-hold avoids conflicting with swipes, what we learned from the Spotify proof of concept.

**How:**
1. Copy [`0000-template.md`](0000-template.md).
2. Name it with the next number and a short title, e.g. `0003-palm-hold-detection.md`.
3. Fill it in (keep it short; half a page is plenty) and include it in your pull request.

## Index

| # | Decision | Author | Status |
|---|---|---|---|
| 0001 | [Split core logic into the `AirDJCore` Swift package](0001-airdjcore-package.md) | Tech Lead | Accepted |
