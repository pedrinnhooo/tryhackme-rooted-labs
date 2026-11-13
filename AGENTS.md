# AGENTS.md

Instructions for AI agents working in this repository.

## Project

Personal TryHackMe / authorized CTF lab notes. Source of truth for each room: `rooms/<sala>/NOTES.md`.

## Layout

```text
AGENTS.md                 ← this file (agent entrypoint)
.cursor/rules/            ← always-on / scoped Cursor rules
.cursor/skills/           ← workflows (load when relevant)
rooms/<sala>/NOTES.md     ← walkthroughs
security-lab.md           ← command cheat sheet
README.md                 ← human overview
```

## Hard rules

1. Authorized targets only (THM labs / explicit scope).
2. Persist every relevant step in `rooms/<sala>/NOTES.md` — not only in chat.
3. Terminal-style notes: `$ command` + **filtered** output + `→ Achado` / `→ Próximo`.
4. Never commit VPN configs, private keys, huge wordlists, or exploit binaries (see `.gitignore`).
5. For room documentation workflow details, follow `.cursor/skills/thm-room-notes/SKILL.md`.

## New room

1. `rooms/<nome-minusculo>/`
2. Create `NOTES.md` from the skill template
3. Update the rooms table in `README.md` when status changes
