# ignite — TryHackMe

```text
[ TARGET ] 10.64.147.255
[ ROOM   ] Ignite
[ GOAL   ] user.txt / root.txt / perguntas da room
[ STATUS ] DONE
```

## 00 — Setup

```console
$ ping -c 2 10.64.147.255
2 packets transmitted, 2 received
```

→ VPN ok, alvo alcançável.

## 01 — Recon (nmap)

```console
$ nmap -sC -sV 10.64.147.255
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
```

→ Achado: só HTTP na 80.
→ Próximo: fingerprint da app + enum dirs.

## 02 — Enum web

```console
$ curl -sI http://10.64.147.255/
HTTP/1.1 200 OK
Server: Apache/2.4.18 (Ubuntu)

$ curl -s http://10.64.147.255/ | rg -i 'title|Version|fuel'
<title>Welcome to FUEL CMS</title>
<h1>Welcome to Fuel CMS</h1>
<h2>Version 1.4</h2>
# admin tip na própria home:
# http://10.64.147.255/fuel
# User name: admin
# Password: admin
```

```console
$ curl -s http://10.64.147.255/robots.txt
User-agent: *
Disallow: /fuel/
```

```console
$ gobuster dir -u http://10.64.147.255 -w tools/seclist/common.txt -t 30 -q
/assets               (Status: 301)
/home                 (Status: 200)
/index.php            (Status: 200)
/robots.txt           (Status: 200)
/offline              (Status: 200)
```

→ Achado: **FUEL CMS 1.4**; `/fuel` no robots + credenciais default publicadas na home.
→ Próximo: login no painel.

## 03 — Admin login

```console
$ # POST login admin:admin → dashboard
final:200 url:http://10.64.147.255/fuel/dashboard
<title>FUEL CMS : Dashboard</title>
<p class="instructions">Welcome to FUEL CMS.</p>
```

→ Achado: `admin:admin` funciona (credencial default do installer).
→ Próximo: ler `/fuel/site_docs` + mapear módulos.

## 04 — Site docs (intel)

```console
$ # http://10.64.147.255/fuel/site_docs
FUEL CMS 1.4  |  PHP/MySQL  |  CodeIgniter

# paths vazados na documentação:
/var/www/html/
/var/www/html/fuel/application/views/_layouts
/var/www/html/fuel/application/config/MY_fuel_layouts.php

# layouts disponíveis em Pages:
main | 301 Redirect | Alias | 404 Error | sitemap.xml | robots.txt | None | Tabbed

# "None" = permite editar o conteúdo livre da página
# Assets → upload em /assets (images, pdf, …)
```

→ Achado: webroot `/var/www/html`; layout **None** = conteúdo livre; Users só gerencia contas (tela atual).
→ Próximo: research CVE do Fuel 1.4 (prioridade RCE).

## 05 — CVEs (research)

```text
[ PRIORIDADE ] CVE-2018-16763  — RCE (crítico, sem auth)
[ SECUNDARIO ] CVE-2018-16762  — SQLi em pages/items
[ SECUNDARIO ] CVE-2021-47980  — blind SQLi no Activity Log (col=), precisa auth

refs:
  https://nvd.nist.gov/vuln/detail/CVE-2018-16763
  https://nvd.nist.gov/vuln/detail/CVE-2021-47980
```

→ Achado: para Ignite, o caminho típico é **CVE-2018-16763**, não o formulário Create Page / Users.
→ Payload/PoC não documentado aqui (Exploit-DB / writeups).

## 06 — Exploitation (CVE-2018-16763)

```text
[ VULNERABILITY ] Fuel CMS 1.4 — Unauthenticated RCE
[ ENDPOINT      ] /fuel/pages/select/?filter=
[ FUNCTION      ] PHP system()
[ TEST          ] id
[ RESULT        ] uid=33(www-data) gid=33(www-data) groups=33(www-data)
[ INITIAL ACCESS] RCE confirmado — execução como www-data
[ STATUS        ] RCE CONFIRMED
```

→ Achado: RCE unauthenticated via `filter=` → shell context `www-data`.

## 07 — Loot via RCE

```console
$ # RCE → pwd
/var/www/html

$ # RCE → ls /home
drwx--x--x  www-data

$ # RCE → ls /home/www-data
-rw-r--r-- 1 root root 34  flag.txt

$ # RCE → cat /home/www-data/flag.txt
6470e394cbf6dab6a91682cc8585059b

$ # RCE → cat /home/www-data/user.txt
# (vazio / não existe)
```

```console
$ # RCE → /etc/passwd (filtrado)
root:x:0:0:root:/root:/bin/bash
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
```

```console
$ # RCE → grep creds em database.php
'hostname' => 'localhost',
'username' => 'root',
'password' => 'mememe',
'database' => 'fuel_schema',
```

```console
$ # RCE → ls config/ (perms)
drwxrwxrwx  …/fuel/application/config/   # world-writable
-rwxrwxrwx  database.php  MY_fuel.php  …
```

→ Achado: **user flag** = `6470e394cbf6dab6a91682cc8585059b`
→ Achado: MySQL local `root:mememe` / DB `fuel_schema`
→ Achado: pasta `config/` **777** — possível vetor de privesc
→ Próximo: privesc → `/root/root.txt`

## 08 — Write check (/tmp)

```console
$ # RCE → write test
20 /tmp/teste.txt
teste-transferencia
```

→ Achado: dá pra escrever em `/tmp` via RCE.

```console
$ # local (rooms/ignite)
$ shasum -a 256 PwnKit.c
716825966ead4bf497a447d9da1df5db2a8d40f8618b4e5a98efc4337874432f  PwnKit.c
```

→ Artefato local `PwnKit.c` presente (hash acima). Transferência/uso do binário: por conta própria — não documentado aqui.

## 09 — Diagnóstico PolicyKit (CVE-2021-4034 / PwnKit)

```text
[ CVE        ] CVE-2021-4034 — PwnKit (LPE)
[ DISTRO     ] Ubuntu 16.04 (contexto do lab)
[ INSTALADO  ] 0.105-14.1ubuntu0.4
[ PATCH      ] 0.105-14.1ubuntu0.5+esm1
[ COMPARAÇÃO ] instalado < patch → potencialmente vulnerável
[ SUID       ] confirmado
[ ROOT       ] ainda não confirmado
[ CONTEXTO   ] RCE Fuel CMS → execução local como www-data
ref: https://ubuntu.com/security/CVE-2021-4034
```

### Pré-requisitos identificados

| Item | Resultado |
| --- | --- |
| Arquitetura | `x86_64` (64 bits) |
| Usuário | `www-data` |
| UID | `33` |
| Binário | `/usr/bin/pkexec` |
| SUID root | Sim |
| PolicyKit | `0.105-14.1ubuntu0.4` |
| Root | Não confirmado |

→ Achado: todos os pré-requisitos de **exposição** listados acima.
→ PwnKit = LPE via `pkexec`; carga de elevação **não** documentada aqui.

## 10 — Root confirmado

```console
$ # LPE → ls /root (filtrado)
-rw-r--r-- 1 root root 34  /root/root.txt
# também: .bash_history .bashrc .cache .nano .profile
```

```console
$ # LPE → cat /root/root.txt
b9bbcb33e11b80be759c4e844862482d
```

```text
[ ROOT   ] confirmado
[ FLAG   ] /root/root.txt
[ STATUS ] DONE
```

→ Room concluída: user + root.

## FLAGS / RESPOSTAS

| Item | Valor |
|---|---|
| user flag | `6470e394cbf6dab6a91682cc8585059b` (`/home/www-data/flag.txt`) |
| root.txt | `b9bbcb33e11b80be759c4e844862482d` |
| CMS | Fuel CMS 1.4 |
| admin panel | `admin` / `admin` |
| MySQL | `root` / `mememe` @ `localhost` / `fuel_schema` |
| webroot | `/var/www/html` |
| RCE user | `www-data` |
| /tmp write | OK |
| polkit | `0.105-14.1ubuntu0.4` (pré-patch → PwnKit) |
| LPE | PwnKit → root confirmado |
