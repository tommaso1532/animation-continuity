# Animation Continuity

An [agent skill](SKILL.md) for reviewing and fixing visual jumps in UI animations.

Checks label and icon alignment, stationary controls, interrupted transitions, and the final handoff to the resting layout. Verification includes frames after animation cleanup.

## Install

```sh
npx skills add tommaso1532/animation-continuity
```

Select your coding agent. Add `-g` for a global installation. Requires Node.js 22.20 or later.

## Use

Review only:

```text
Use $animation-continuity to review this chip-to-flyout transition.
Check opening, closing, interruptions, and frames after cleanup.
Report visual jumps with evidence.
```

Fix a specific problem:

```text
Use $animation-continuity to fix the close icon jumping when the
flyout finishes closing. Preserve the current morph and press feedback.
Verify the icon, label, and neighboring controls through cleanup.
```

## Scope

Fixes need source access. Verification needs a running UI, recording, or frame timeline.

Used on a React Native Web prototype. Native implementations remain unverified. Visual continuity and CLS are separate checks.

[MIT license](LICENSE).
