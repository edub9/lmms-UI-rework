# Piano Roll visual refresh — implementation plan

Branch: `feature/piano-roll-visual-refresh`

## Scope
First pass is rendering/theme only; preserve MIDI editing, recording, note positioning, snap, quantization, undo, and playback.

## Verified entry point
- `src/gui/editors/PianoRoll.cpp`
- `PianoRoll::drawNoteRect` draws MIDI note gradients, borders, labels and resize end marks.
- Note rendering already receives note/selection colors; prefer changing theme-driven colors instead of replacing interaction logic.

## Visual specification
- Charcoal background with subtly alternating semitone rows.
- Clear bar and beat lines with restrained contrast.
- Blue notes, brighter blue selection outlines, readable note labels.
- Matching piano keyboard and controller/velocity lane.
- Keep note volume/pan gradient cues visible.
- Preserve accessibility and sufficient contrast.

## Delivery sequence
1. Locate the theme definitions and piano-roll paint paths.
2. Implement palette changes with the smallest feasible diff.
3. Verify note drawing, selection, resize cues, velocity/controller lane.
4. Run Linux x86-64 build and regression tests.
5. Review screenshot against approved concept.

## Acceptance criteria
- No changes to MIDI event data or editor input behavior.
- Notes remain distinguishable by selection and velocity.
- No compile errors or visual regressions at standard/high DPI.
- Build and tests must be reported as unverified until run.
