# Docked workspace implementation plan

Target: FL Studio-inspired docked LMMS interface, preserving existing audio/MIDI functionality.

## Existing architecture

- `src/gui/MainWindow.cpp` creates a horizontal `QSplitter` and a `QMdiArea` workspace.
- Existing editor windows are added through `MainWindow::addSubWindow`.
- The current theme is `data/themes/default/style.css`.
- `.github/workflows/build.yml` already packages Linux AppImage artifacts.

## Intended layout

- Top: transport, tempo, time signature and global toolbar.
- Left: resizable instrument/sample/project browser.
- Center: arrangement/song editor as primary pane.
- Bottom: resizable piano roll / pattern editor tabs.
- Right or bottom-right: resizable mixer, with meters and effects.
- Optional undock/float action for each editor.

## Implementation sequence

1. Introduce a persistent dock host with Qt dock widgets or nested splitters, and move existing editor widgets into the host without recreating their underlying models.
2. Preserve floating window behavior, keyboard shortcuts, and current project files.
3. Save/restore dock geometry and visibility; provide a Reset Workspace command.
4. Test audio playback, piano-roll editing, automation, mixer, browser and window focus.
5. Build Linux x86_64 AppImage using the existing GitHub Actions workflow, download the build artifact and smoke-test it before publishing a release.

This document is a design/implementation plan, not a claim that docking or an AppImage has been completed.
