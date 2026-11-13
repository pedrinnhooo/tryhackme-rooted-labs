# tryhackme lab — project guide

Repo pessoal de labs (TryHackMe / CTF autorizado).

## O que usar pra quê

| Arquivo | Função |
|---|---|
| `.cursor/rules/ctf-room-docs.mdc` | **Rule** — sempre ativa: como documentar cada room |
| `docs/SKILLS/thm-room-notes.md` | **Skill** — template, filtro de saída, exemplos |
| `security-lab.md` | Arsenal de comandos / cheat sheet |
| `rooms/<sala>/NOTES.md` | Passo a passo da room (fonte da verdade) |

`CLAUDE.MD` = mapa do projeto.  
`SKILLS` = como executar o workflow.  
`.cursor/rules` = o que o agente **sempre** deve seguir.

## Convenções rápidas

- Room nova → `rooms/<nome-minusculo>/` + `NOTES.md`
- Todo comando relevante entra no NOTES em estilo terminal, com saída **filtrada**
- Dumps brutos opcionais ao lado (`nmap.txt`, etc.)
- Só alvos autorizados / escopo do lab
