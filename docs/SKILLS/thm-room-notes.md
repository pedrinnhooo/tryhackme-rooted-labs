---
name: thm-room-notes
description: >-
  Documenta rooms TryHackMe/CTF com NOTES.md em estilo terminal, saídas filtradas
  e passo a passo até o objetivo. Use ao iniciar/continuar rooms, recon, enum,
  hydra, nmap, gobuster, SSH/FTP, ou quando o usuário pedir para anotar o lab.
---

# THM Room Notes

## Objetivo

Cada room vira um log legível: comando → resposta mascarada → achado → próximo passo.

## Estrutura da pasta

```text
rooms/<sala>/
├── NOTES.md          # obrigatório — walkthrough terminal
├── nmap.txt          # opcional — dump bruto se precisar revisar
├── gobuster.txt
├── hydra.txt
└── ...               # loot (hashes, keys, screenshots)
```

Nome da pasta: **minúsculo**, hífen se precisar (`basic-pentest`, `ignite`).

## Template do NOTES.md

```markdown
# <sala> — TryHackMe

```text
[ TARGET ] <IP>
[ ROOM   ] <nome>
[ GOAL   ] user.txt / root.txt / perguntas da room
[ STATUS ] IN PROGRESS | DONE
```

## 00 — Setup

```console
$ sudo openvpn <arquivo>.ovpn
[+] tun0 up
```

```console
$ ping -c 2 <IP>
2 packets transmitted, 2 received
```

→ VPN ok, alvo alcançável.

## 01 — Recon (nmap)

```console
$ nmap -sC -sV <IP>
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH …
80/tcp open  http    Apache …
```

→ Achado: …
→ Próximo: …

## 02 — Enum web

```console
$ gobuster dir -u http://<IP> -w tools/seclist/common.txt
/admin                (Status: 200)
/robots.txt           (Status: 200)
```

→ Achado: …
→ Próximo: …

## … — continuar até o objetivo

## FLAGS / RESPOSTAS

| Item | Valor |
|---|---|
| user.txt | `…` |
| root.txt | `…` |
```

## Formato de cada passo

1. Título numerado (`## 01 — …`)
2. Bloco `console` com `$ comando` + saída **filtrada**
3. Linhas `→ Achado:` / `→ Próximo:`
4. Se salvar dump bruto, referenciar: `(raw: nmap.txt)`

## Filtro de saída (máscara)

Manter só o que muda a decisão:

| Tool | Manter | Cortar |
|---|---|---|
| nmap | tabela PORT/STATE/SERVICE/VERSION, scripts úteis | banner Nmap, timing, “Scanning…”, hostkeys longas se irrelevantes |
| gobuster | path + status (+ size se útil) | progress bar, banners |
| hydra | linha de sucesso / login encontrado | cada tentativa falha |
| curl | status, headers interessantes, trecho do body | HTML inteiro |
| nc | banner / resposta do serviço | ruído de conexão |
| ssh/ftp | login ok, pwd, ls relevante | MOTD longo |
| find/SUID | paths interessantes | Permission denied em massa (já redirecionado) |
| stego/file | tipo, hash curto, conteúdo recuperado | dumps hex enormes |

Regras gerais:

- Preferir **resumo denso** a dump completo
- Credenciais de lab THM podem ir no NOTES; **nunca** VPN/pessoais
- Se a saída for enorme: salvar raw em arquivo e no NOTES só o highlight

## Exemplo bom vs ruim

**Ruim**

```console
$ nmap -sC -sV 10.1.1.1
Starting Nmap 7.99 …
Nmap scan report for …
Host is up (0.04s latency).
Not shown: 997 closed ports
...
Nmap done: 1 IP address (1 host up) scanned in 12.3 seconds
```

**Bom**

```console
$ nmap -sC -sV 10.1.1.1
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1
80/tcp open  http    Apache httpd 2.4.29
|_http-title: Fuel CMS
```

→ HTTP + Fuel CMS. Próximo: enum em `/` e pesquisa CVE do CMS.

## Checklist do agente

- [ ] Pasta `rooms/<sala>/` existe
- [ ] `NOTES.md` atualizado neste passo
- [ ] Saída filtrada (sem banner/progresso)
- [ ] Achado + próximo passo escritos
- [ ] Flags/respostas no final quando obtidas
