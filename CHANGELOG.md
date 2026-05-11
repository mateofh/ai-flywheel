# Changelog

## [Unreleased] — 2026-05-11

### Changed
- **Renamed `/level-up` command to `/flywheel`** to align the slash command with the brand
  already used in the repo name, config dir (`~/.config/ai-flywheel/`), and dashboard outputs
  (`ai-flywheel-results.html`).

### Migration
Existing users who installed `/level-up` before this change need to:

```bash
# Remove the old command file
rm ~/.claude/commands/level-up.md

# Install the renamed command
curl -sL https://raw.githubusercontent.com/mateofh/ai-flywheel/main/.claude/commands/flywheel.md \
  -o ~/.claude/commands/flywheel.md
```

Run history in `~/.config/ai-flywheel/runs/*.json` is preserved — `/flywheel --history`
still shows your past runs.

### Preserved
- Historical attribution to David Arnoux's original `/level-up` (see README footer).
- Config dir, HTML output paths, run history JSON schema — all unchanged.

## Previous

Earlier versions had no formal changelog. v2.1 introduced vitality scoring, radar charts,
history tracking, dead-weight detection, and tier-specific deliverables (see README).
