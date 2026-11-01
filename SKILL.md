---
name: animation-continuity
description: Preserve geometric continuity when implementing, iterating on, reviewing, or debugging UI animations and transitions, including motion parameter tuning. Check label and icon anchors, layout changes, and final-frame snaps during handoff to the resting view and after animation cleanup. Applies to web and native UI.
license: MIT
---

# Animation Continuity

Preserve the requested motion and the geometry of stationary content throughout the transition and its completion. Apply this alongside the project's motion conventions and verification workflow, using its existing framework and tools.

## Establish the geometry

Before editing, identify the affected elements and their observable constraints. Reuse unchanged constraints during subsequent iterations:

- Which surface, labels, and icons move, and on which axes? Which remain stationary?
- What is each transform's anchor: the trigger, container, individual icon, visible text, or baseline? A text field's center and its text's center are different points.
- What geometry must the final state have, including dimensions, spacing, wrapping, and alignment with neighboring content?
- Which elements remain visible, become covered, or fade? Distinguish a stationary element being uncovered from one fading back into existence.

Derive these constraints from the user's request and established design. Add translations, directional hints, or group scaling only when that design calls for them. Generic animation recipes are subordinate to these constraints. A request for blur, opacity, or scale around a fixed anchor preserves that anchor throughout.

## Keep animated and resting geometry consistent

- Share the authoritative content layout between real elements and animation copies where possible. Match fonts, line height, actual whitespace, padding, borders, gaps, shrink behavior, and wrapping. Reconstructing a similar-looking label can change its glyph position even when its outer box matches.
- Measure source and destination in the same coordinate system. Account for scroll, safe areas, parent transforms, and display scale when relevant. Keep targets current when selections or content change; stabilize layout-affecting resources before measuring.
- Treat surface morphing and content motion separately when their constraints differ. Resizing or scaling a parent can move or distort its children. Check the composition of parent and child transforms against the required anchors.
- Preserve current position and, where applicable, velocity when an animation is interrupted or retargeted. Use the framework's actual completion and cancellation semantics. A guessed timeout does not establish that a spring has settled or the destination layout is ready.
- Make the last visible animated geometry match the first visible resting geometry. Cleanup, unmounting an overlay, restoring a hidden original, resetting transforms, or applying a final class/state must preserve that match. Include any press-release scale that overlaps the handoff.

Correct geometry mismatches at their source before tuning timing. Blur, a longer easing tail, or compositor promotion cannot establish that two layouts agree. Distinguish actual layout changes, visual displacement from transforms, and rasterization differences when diagnosing a reported shift.

## Verify the final handoff

Use the repository's verification workflow and reuse evidence whose covered behavior remains unchanged. Keep checks focused on the changed transition; use an applicable delivery gate once at its prescribed stage.

1. Exercise the affected interaction in the real UI, including opening and closing when both exist. Inspect it at normal speed and use a short recording or controllable timeline to examine frames as needed.
2. Observe the last visible animated frames, the actual handoff to the resting view, and at least two rendered frames after animation cleanup. Continue through any deferred layout or visibility update. A selected intermediate timestamp or an isolated final screenshot leaves the handoff unverified.
3. Compare the same visible features in the same screen coordinate system. For text, inspect glyph position or baseline when the surrounding box is insufficient; for icons, use their visible geometry when the view box hides an offset. Check stationary neighbors as well as the animated element. Apply only constraints appropriate to that feature: intentional scaling can change its size while its required anchor stays fixed.
4. Check geometry and visibility together. A fixed label that disappears behind an overlay and pops back after cleanup still violates a requirement to remain visible or be progressively uncovered.
5. Check supported reversal or retargeting when the changed path can be interrupted. Exercise a different content width, fractional scale, viewport, or reduced-motion path when the edit affects that behavior; preserve intentional travel and overshoot.

Choose a small comparison tolerance justified by the renderer's rounding and device scale, and record its units. A visible step remains a defect even if a loose numeric assertion passes.

When adding or adapting a regression test, observe rendered behavior through completion and cleanup. Obtain expected resting geometry from the real destination element, independently of the animation's interpolation formula. Tie sampling to the observed lifecycle or settled UI state; a hardcoded delay alone cannot prove the terminal frames were covered. Use the smallest test that detects the reported defect, and retain visual inspection where geometry assertions cannot establish visibility or perceived motion.

## Completion

Report the affected transition, the relevant geometric constraints, and the evidence covering the final handoff and subsequent frames. A build, a frame-rate measurement, or tests that only cover container bounds do not establish continuity of text and icons. If the UI could not be exercised or cleanup was not observed, state that geometric continuity remains unverified. Preserve the requested scope: a review produces findings; implementation follows the existing authorization and delivery rules.
