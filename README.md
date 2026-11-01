# Animation Continuity

A focused agent skill for AI-assisted UI prototyping. Detect and fix visual jumps during transitions, interruptions, and the return to the resting layout.

For product designers prototyping with AI, design engineers, and frontend developers. Give your agent a concrete problem such as "the close icon jumps when the flyout finishes closing" and use this skill to turn it into observable constraints and a focused verification.

## What it checks

- Label and icon anchors, including glyph positions and visible icon geometry.
- Consistent layout between the real component and its animation copy.
- Surface morphing and content motion, including children that should stay still.
- The last animated frame, the first resting frame, and frames after cleanup.
- Interrupted or reversed transitions, overlapping press feedback, scroll offsets, and fractional display scales when relevant.

The skill preserves the intended design. It does not prescribe a library, easing curve, animation duration, or amount of bounce. Use it alongside your project's motion, performance, and accessibility guidance.

## Install

With the [skills CLI](https://github.com/vercel-labs/skills), run this from your project:

```sh
npx skills add tommaso1532/animation-continuity
```

Select the coding agent you use. Add `-g` to install across projects instead of only the current project. The CLI requires Node.js 22.20 or later.

For a manual installation, place this repository's `SKILL.md` and `agents/` directory together in an `animation-continuity` folder inside your agent's supported skills directory. Other agents can read `SKILL.md`; `agents/openai.yaml` supplies Codex-specific UI metadata.

## Use it

### Review a transition

```text
Use $animation-continuity to review this chip-to-flyout transition.
Review only. Identify what should stay fixed, inspect opening and
closing through cleanup, and report any visual jumps with evidence.
```

### Fix a visual jump

```text
Use $animation-continuity to fix the close icon jumping at the end
of the flyout's closing animation. Preserve the current morph and
press feedback. Verify the icon, label, and neighboring controls
through the handoff and at least two rendered frames after cleanup.
```

### Check interrupted motion

```text
Use $animation-continuity to check what happens when I reopen the
flyout before it finishes closing. Review only. Compare the visible
geometry before and after reversal, then follow it to its resting state.
```

A review request produces findings. A fix request authorizes implementation within the requested scope. `$animation-continuity` is the skill identifier; the display name is **Animation Continuity**. Invocation syntax can differ between agents.

## What the agent needs

The agent needs source access to implement changes and a way to observe motion, such as a browser, simulator, recording, or controllable frame timeline. A single screenshot can reveal a layout mismatch, but cannot establish continuity over time. When runtime evidence is unavailable, the agent should report what remains unverified.

The package contains instructions and optional UI metadata. It does not bundle an animation engine, browser automation, a test runner, or a Figma plugin. Designers working from recordings can use it as a review checklist; implementation requires access to the prototype's code.

## Validation and limits

The workflow was used while debugging a filter-chip/flyout prototype built with React Native Web. That session compared the animated and resting close-icon centers, corrected their different layout rules, and checked three rendered frames after the animated copy was removed. Targeted checks also covered chip resizing, press feedback, and interrupted morphs.

This is evidence from one web prototype, not a controlled comparison of agent performance or a guarantee that every visual jump will be detected. Web and native UI share the geometric principles, but native implementations and other frameworks have not been validated by this project. Agent behavior and available tools affect the result.

The post-cleanup frame count is a minimum observation window. Continue through deferred layout or visibility updates when the component has them.

### Visual continuity and CLS

This skill evaluates perceived visual continuity. Cumulative Layout Shift is a separate browser metric with its own inclusion rules. For example, shifts within 500 ms of qualifying user input can be excluded from CLS even when someone can see the jump. Assess visual continuity and CLS separately. See [web.dev's CLS documentation](https://web.dev/articles/cls).

## Related reading

- [Apple: Designing Fluid Interfaces](https://developer.apple.com/videos/play/wwdc2018/803/), for continuous interaction and interruptible motion.
- [Motion: Layout animations](https://motion.dev/docs/react-layout-animations), for scroll measurements, child distortion, and scrollbar-induced layout changes.

These resources provide background. The skill focuses on checking geometric continuity through the final return to the resting layout.

## License

[MIT](LICENSE).
