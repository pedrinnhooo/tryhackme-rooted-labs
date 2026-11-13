# tryhackme-rooted-labs

Personal lab notes from rooted [TryHackMe](https://tryhackme.com) rooms.

## Structure

```text
AGENTS.md                      # agent entrypoint (AI)
.cursor/rules/                 # Cursor project rules
.cursor/skills/thm-room-notes/ # room-notes workflow skill
rooms/<sala>/NOTES.md          # walkthroughs (source of truth)
security-lab.md                # command cheat sheet
docs/bug-bounty/               # misc research notes
tools/ · vpn/                  # local only — not committed
```

## Rooms

| Room | Status |
|---|---|
| [ignite](rooms/ignite/NOTES.md) | DONE |
| [brooklyn-nine-nine](rooms/brooklyn-nine-nine/NOTES.md) | DONE |
| [overpass](rooms/overpass/NOTES.md) | DONE |
| [basic-pentest](rooms/basic-pentest/NOTES.md) | DONE |
| [chill-hack](rooms/chill-hack/NOTES.md) | DONE |
| [mustacchio](rooms/mustacchio/NOTES.md) | DONE |
| [root-me](rooms/root-me/NOTES.md) | DONE |

## Conventions

- Authorized labs only.
- VPN, keys, and large wordlists stay out of git (`.gitignore`).
- Agent instructions live in `AGENTS.md` + `.cursor/`
