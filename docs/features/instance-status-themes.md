# Instance age, state, and themes

- [x] Carry EC2 launch time through the inventory merge.
- [x] Show age in the instance list and detail sidebar. Show EC2 state and SSM readiness separately.
- [x] Add per-model theme presets through YAML and a command flag. Preserve the default palette.
- [x] Check age boundaries, inventory merge, config persistence, rendering, and theme isolation.
- [x] Run the Go tests, race detector, static checks, and build. Review the diff and document limitations.

Age means elapsed time since the EC2 launch time reported by AWS. It is not operating-system uptime.
Missing timestamps show `—`. Future timestamps show `0m`.
Themes use semantic styles per model, without global mutable styles.

## Decisions

- EC2 launch time flows through the existing inventory merge. No extra AWS request is needed.
- Each model owns its theme styles. A command override does not modify the store.
- The IP column uses the existing IP value and appears at a list width of 68 columns.
- The list keeps age, state, and SSM readiness separate. Selected rows retain status colors.

## Limits and follow-up

- AWS permission errors can leave age and state unknown in SSM-only mode.
- Compact terminals truncate row text. Wider terminals show the separate columns.
- Age reflects the latest screen render. No background age timer is added.
- Existing help and preflight screens can wrap in very narrow terminals.
- First follow-up: check the layout with a real fleet and a preferred terminal palette.

## Verification

- The full Go test suite and race detector passed.
- Static checks and the local binary build passed.
- Render tests cover all list widths from 34 to 200 and terminal widths from 60 to 200.
- A real terminal demo passed at 130×25 with Dracula and 96×24 with Nord.
- The wide demo shows the IP column. The narrow demo hides it and keeps age and state visible.
- The terminal respects `NO_COLOR`. Unset it to use theme colors.
- No live AWS requests were needed for these checks.
