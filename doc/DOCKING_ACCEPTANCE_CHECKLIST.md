# Docking acceptance and regression checklist

This checklist is a release gate, not evidence that any test has passed.

## Architecture constraints verified in MainWindow.cpp

- MainWindow creates a nested QMainWindow (`editorDockHost`) whose central widget is the existing `MovableQMdiArea`.
- `finalize()` adds automation, pattern, piano roll and song editor widgets through `addWindowedWidget()`.
- `addWindowedWidget()` wraps each editor in a custom `SubWindow`, connects widget destruction to window deletion, and adds it to the MDI workspace.
- `saveWidgetState()` and `restoreWidgetState()` assume `SubWindow` ownership. Reparenting editors directly into `QDockWidget` without adapting these functions risks broken project/window state.
- Existing keyboard shortcuts include Ctrl+1 song editor, Ctrl+2 pattern editor, Ctrl+3 piano roll, Ctrl+4 automation, Ctrl+5 mixer.

## Required implementation order

1. Introduce an explicit, opt-in docking mode; preserve the existing MDI layout by default.
2. Implement editor-host migration with a single owner per editor. Avoid destroying editor widgets or recreating song/audio models when switching modes.
3. Update window-state serialization and editor show/hide commands to handle both `SubWindow` and `QDockWidget` containers.
4. Dock song editor as the primary central workspace; dock piano roll, pattern editor, mixer, and automation in independently movable/resizable panes. Integrate browser sizing without duplicating the sidebar.
5. Save and restore dock state using stable object names and `QMainWindow::saveState/restoreState`; add Reset Workspace.
6. Validate floating, redocking, tabbing, splitting, shortcut focus and visibility; ensure switching back to MDI does not lose editor state.

## Manual smoke tests (all currently unverified)

- [ ] Default startup still uses legacy MDI layout.
- [ ] Opt-in docking shows timeline, browser, piano roll and mixer.
- [ ] Each dock can resize, rearrange, float and reattach.
- [ ] Workspace state survives restart and Reset Workspace restores defaults.
- [ ] Ctrl+1 through Ctrl+5 activate correct editors in both modes.
- [ ] Piano-roll note insert/move/delete and undo/redo work.
- [ ] Song timeline clip edit and playback work.
- [ ] Mixer levels, effect controls and automation work.
- [ ] Opening/saving existing MMP/MMPZ projects works in both modes.
- [ ] Closing and reopening docks does not crash or delete models.
- [ ] Linux x86_64 AppImage builds and launches on a clean system.

Do not change the default layout or mark the PR ready until these tests pass.
