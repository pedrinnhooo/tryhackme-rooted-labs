# mustacchio — TryHackMe

```text
[ TARGET ] 10.64.163.102
[ ROOM   ] Mustacchio
[ GOAL   ] user.txt / root.txt
[ STATUS ] PARTIAL — creds + vetor XXE/comment; flags não salvas
```

## 01 — Credenciais (users.bak)

```console
$ file users.bak
SQLite 3.x database

$ strings users.bak
CREATE TABLE users(username text NOT NULL, password text NOT NULL)
admin 1868e36a6d2b17d4c2745f1659433a54d4bc5f4b
```

```console
$ # hash SHA1 crackeado
1868e36a6d2b17d4c2745f1659433a54d4bc5f4b  →  bulldog19
```

→ Achado: `admin:bulldog19`. (raw: `users.bak`, `recuperado.txt`)
→ Próximo: login no serviço web.

## 02 — Acesso admin + ataque

```console
$ # painel em :8567 (nota do lab)
# login admin → add script comment for attack
```

→ Achado: após login, vetor via **comentário/script** (XXE/injeção anotada no recuperado).
→ Próximo: explorar até shell → user/root flags.

## FLAGS / RESPOSTAS

| Item | Valor |
|---|---|
| admin | `admin` / `bulldog19` |
| user.txt | *(completar)* |
| root.txt | *(completar)* |

## Artefatos

`users.bak` · `recuperado.txt`
