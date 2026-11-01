# Animation Continuity

An [agent skill](SKILL.md) for reviewing and fixing visual jumps in UI animations.

Use it when a label jumps at the end of a morph, an icon lands in the wrong place, or neighboring controls shift as a flyout closes. It works with your existing animation code and preserves the intended motion.

## What it checks

- Text and icon positions, including baselines and visible shapes.
- Matching padding, borders, spacing, and scale between a component and its animated copy.
- Opening, closing, and reversals, including overlapping press feedback, scroll offsets, and zoom.
- The last animated state, the first resting state, and frames after animation cleanup.

The agent checks layout and anchors before adjusting easing, blur, or spring settings.

## Install

```sh
npx skills add tommaso1532/animation-continuity
```

Select your coding agent. Add `-g` for a global installation. Requires Node.js 22.20 or later.

## Use

Tell the agent which elements should move and which should stay fixed. Provide source access for fixes and a running UI or recording to verify motion.

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

[MIT license](LICENSE).
