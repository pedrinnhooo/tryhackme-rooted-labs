# 🥷 CYBERSECURITY ARSENAL

### `~/security-lab` — Personal Toolkit & Command Reference

```text
   ██████╗██╗   ██╗██████╗ ███████╗██████╗
  ██╔════╝╚██╗ ██╔╝██╔══██╗██╔════╝██╔══██╗
  ██║      ╚████╔╝ ██████╔╝█████╗  ██████╔╝
  ██║       ╚██╔╝  ██╔═══╝ ██╔══╝  ██╔══██╗
  ╚██████╗   ██║   ██║     ███████╗██║  ██║
   ╚═════╝   ╚═╝   ╚═╝     ╚══════╝╚═╝  ╚═╝

  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓
  [ SYSTEM  ] ONLINE
  [ MODE    ] LEARNING & AUTHORIZED TESTING
  [ ENV     ] macOS / Linux / TryHackMe
  [ STATUS  ] KEEP LEARNING
  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓
```

> **Objetivo:** reunir comandos, ferramentas e técnicas essenciais de cibersegurança em um único lugar para consulta rápida durante estudos, CTFs, análise de sistemas e laboratórios de pentest.

```text
[+] Knowledge is the first tool.
[+] Enumeration before exploitation.
[+] Evidence before conclusions.
[+] Authorization before testing.
```

---

# 📑 ÍNDICE

```text
┌─────────────────────────────────────────────────────────┐
│  NAVIGATION MAP — jump to any module                    │
└─────────────────────────────────────────────────────────┘
```

### `~` Fundamentos

| # | Módulo | Atalho |
|---|---|---|
| 01 | [Terminal & Arquivos](#01-terminal) | navegação, grep, find |
| 02 | [Processos & Portas](#02-processos) | ps, lsof, kill |
| 03 | [VPN & Conectividade](#03-vpn) | OpenVPN, ping, rotas |
| 04 | [Permissões & Hashes](#04-permissoes) | chmod, whoami, shasum |

### `~` Reconhecimento

| # | Módulo | Atalho |
|---|---|---|
| 05 | [Nmap](#05-nmap) | portas, serviços, scripts |
| 06 | [Enumeração Web](#06-web-enum) | gobuster, wordlists |
| 07 | [DNS & Rede](#07-dns) | dig, whois, protocolos |
| 08 | [HTTP & APIs](#08-http) | curl, status codes |

### `~` Acesso & Autenticação

| # | Módulo | Atalho |
|---|---|---|
| 09 | [Netcat & Conexões](#09-netcat) | listener, probe |
| 10 | [Hydra](#10-hydra) | auditoria de senhas |
| 11 | [Acesso Remoto](#11-acesso-remoto) | SSH / FTP / SFTP / SCP |
| 12 | [Identificar Serviços](#12-servicos) | credenciais → serviço |

### `~` Análise & Investigação

| # | Módulo | Atalho |
|---|---|---|
| 13 | [Esteganografia](#13-stego) | stegify, strings, exif |
| 14 | [Análise de Arquivos](#14-arquivos) | file, hashes, xxd |
| 15 | [Logs & Investigação](#15-logs) | grep, tail, timeline |

### `~` Workflows & Meta

| # | Módulo | Atalho |
|---|---|---|
| 16 | [Fluxo de Trabalho CTF](#16-fluxo-ctf) | playbook passo a passo |
| 17 | [Checklist de Pentest](#17-checklist) | fases 1–4 |
| 18 | [Google Bug Hunters](#18-bug-hunters) | Android / mobile VRP |
| 19 | [Organização do Lab](#19-lab) | estrutura de pastas |
| 20 | [Regras de Segurança](#20-regras) | autorização first |

[↑ voltar ao índice](#indice)

---

# 01 — TERMINAL & ARQUIVOS

```text
[ MODULE 01 ] FILESYSTEM OPS
[ TAGS      ] pwd · ls · find · grep · head · tail
```

### ▸ Navegação básica

```bash
# Mostra o diretório atual
pwd

# Lista arquivos e diretórios, incluindo arquivos ocultos
ls -lah

# Exibe o conteúdo de um diretório específico
ls -lah ~/Downloads

# Cria um diretório e seus pais, se necessário
mkdir -p minha_pasta/subpasta

# Cria um arquivo de teste
touch minha_pasta/arquivo.txt

# Grava conteúdo em um arquivo, substituindo o conteúdo anterior
echo "Conteúdo de exemplo." > minha_pasta/arquivo.txt

# Acrescenta conteúdo sem apagar o que já existe
echo "Nova linha." >> minha_pasta/arquivo.txt

# Exibe o conteúdo do arquivo
cat minha_pasta/arquivo.txt

# Exibe o conteúdo página por página
less minha_pasta/arquivo.txt

# Copia um arquivo
cp minha_pasta/arquivo.txt copia.txt

# Move ou renomeia um arquivo
mv copia.txt arquivo_renomeado.txt

# Procura arquivos por nome
find . -type f -name "*.txt"
```

### ▸ Pesquisa de conteúdo

```bash
# Procura uma palavra em um arquivo
grep "password" arquivo.txt

# Pesquisa ignorando maiúsculas e minúsculas
grep -i "error" arquivo.txt

# Pesquisa recursivamente dentro de um diretório
grep -Rni "TODO" ./projeto

# Conta linhas, palavras e caracteres
wc -l arquivo.txt

# Exibe as primeiras 20 linhas
head -n 20 arquivo.txt

# Exibe as últimas 20 linhas
tail -n 20 arquivo.txt

# Acompanha um arquivo de log em tempo real
tail -f aplicacao.log

# Mostra o tipo real de um arquivo
file arquivo_desconhecido
```

### ▸ Remoção de aplicativos no macOS

> ⚠ `rm -rf` remove arquivos recursivamente **sem pedir confirmação**. Confira o caminho antes de executar.

```bash
# Exemplo: remover um aplicativo específico
sudo rm -rf "/APPs/<appName>.app"
```

Não execute comandos de remoção com caminhos não verificados ou variáveis vazias.

[↑ índice](#indice)

---

# 02 — PROCESSOS & PORTAS

```text
[ MODULE 02 ] PROCESS & SOCKET INTEL
[ TAGS      ] ps · pgrep · kill · lsof · ss · netstat
```

### ▸ Monitoramento de processos

```bash
# Lista os processos em execução
ps aux

# Procura um processo pelo nome
pgrep -fl netcat

# Mostra informações de um processo específico
ps -p 45588 -o pid,ppid,stat,command

# Encerra um processo pelo PID
kill 45588

# Solicita encerramento de um processo pelo nome
killall openvpn
```

### ▸ Identificar portas ocupadas

```bash
# Verifica quem está usando a porta TCP 7000
lsof -nP -iTCP:7000

# Verifica quem está usando a porta TCP 4444
lsof -nP -iTCP:4444

# Lista conexões TCP e UDP
lsof -nP -i

# Lista sockets de rede no Linux
ss -tulnp

# Lista portas em escuta no macOS
netstat -an | grep LISTEN
```

### ▸ Liberar uma porta

```bash
# 1. Identifica o processo
lsof -nP -iTCP:4444

# 2. Encerra o PID encontrado
kill 45588

# 3. Confirma se a porta foi liberada
lsof -nP -iTCP:4444
```

Se o processo não encerrar, confirme que o PID continua sendo o mesmo antes de considerar `kill -9`.

[↑ índice](#indice)

---

# 03 — VPN & CONECTIVIDADE

```text
[ MODULE 03 ] TUNNEL & REACHABILITY
[ TAGS      ] openvpn · ifconfig · ping · route
```

### ▸ OpenVPN — TryHackMe

```bash
# Conecta usando o arquivo de configuração fornecido
sudo openvpn us-east-1-padin-regular.ovpn

# Encerra os processos OpenVPN
sudo killall openvpn
```

> Execute o OpenVPN em um terminal dedicado. Confira se o arquivo `.ovpn` corresponde à sua configuração atual.

### ▸ Diagnóstico de rede

```bash
# Mostra as interfaces e os endereços IP no macOS
ifconfig

# Mostra o IP de uma interface específica
ipconfig getifaddr en0

# Mostra a rota até a máquina do laboratório
route -n get 10.65.170.96

# Testa a conectividade com um endereço
ping -c 4 10.65.170.96

# Consulta as informações de roteamento no Linux
ip route

# Mostra as conexões de rede do sistema
netstat -an
```

### ▸ Conceitos importantes

| Token | Significado |
|---|---|
| `127.0.0.1` | loopback |
| `192.168.x.x` | rede local privada comum |
| `10.x.x.x` | rede privada (labs / corporativo) |
| `tun` / `utun` | interfaces tipicamente ligadas a VPN |

> Uma rota ativa **não garante** que a máquina remota consiga iniciar conexões de volta para você.

[↑ índice](#indice)

---

# 04 — PERMISSÕES & HASHES

```text
[ MODULE 04 ] IDENTITY · ACL · INTEGRITY
[ TAGS      ] chmod · whoami · id · shasum · sha256sum
```

### ▸ Permissões Linux/macOS

```bash
# Exibe permissões e proprietário de um arquivo
ls -l arquivo.txt

# Altera permissões para leitura e escrita do proprietário
chmod 600 arquivo.txt

# Torna um script executável pelo proprietário
chmod u+x script.sh

# Exibe o usuário atual
whoami

# Exibe UID, GID e grupos
id

# Exibe informações de identidade do usuário
id -un
```

Evite conceder permissões amplas, como `777`, sem uma necessidade clara.

### ▸ Hashes de arquivos

```bash
# SHA-256 no macOS
shasum -a 256 arquivo.txt

# SHA-256 no Linux
sha256sum arquivo.txt

# Compara o hash calculado com um valor confiável
shasum -a 256 arquivo.txt
```

A comparação só é útil se o hash de referência vier de uma fonte confiável.

[↑ índice](#indice)

---

# 05 — RECONHECIMENTO COM NMAP

```text
[ MODULE 05 ] NETWORK RECON
[ TAGS      ] -sV · -sC · -p- · -oA · -sn
```

> Use somente contra máquinas próprias ou alvos **autorizados**.

### ▸ Descoberta de serviços

```bash
# Descobre portas TCP comuns
nmap 10.65.170.96

# Identifica versões dos serviços
nmap -sV 10.65.170.96

# Executa scripts padrão e identifica versões
nmap -sC -sV 10.65.170.96

# Varre todas as portas TCP
nmap -p- 10.65.170.96

# Varre todas as portas TCP e identifica serviços
nmap -p- -sV 10.65.170.96

# Salva os resultados em arquivos
nmap -sC -sV -oA nmap_resultado 10.65.170.96

# Examina portas específicas
nmap -p 22,80,443 10.65.170.96

# Verifica apenas se o host parece estar ativo
nmap -sn 10.65.170.96
```

### ▸ Interpretando resultados

| Estado | Significado |
|---|---|
| `open` | Serviço aceitando conexões na porta |
| `closed` | Host respondeu, mas nada escuta nessa porta |
| `filtered` | Estado indeterminado (filtro / sem resposta) |

```text
FLUXO RECOMENDADO
  portas → serviços → versões → vulns relevantes
```

[↑ índice](#indice)

---

# 06 — ENUMERAÇÃO WEB

```text
[ MODULE 06 ] WEB SURFACE MAPPING
[ TAGS      ] gobuster · wordlists · status codes
```

### ▸ Gobuster

Ferramenta para descobrir diretórios e arquivos por meio de wordlists.

```bash
# Enumera diretórios e arquivos de um site autorizado
gobuster dir \
  -u http://10.65.191.241 \
  -w tools/seclist/common.txt

# Enumera com extensões comuns de arquivos web
gobuster dir \
  -u http://10.65.156.192 \
  -w tools/seclist/common.txt \
  -x php,html,txt

# Salva os resultados em arquivo
gobuster dir \
  -u http://10.65.156.192 \
  -w tools/seclist/common.txt \
  -o gobuster.txt

# Define códigos HTTP relevantes para exibição
gobuster dir \
  -u http://10.65.156.192 \
  -w tools/seclist/common.txt \
  -s 200,204,301,302,307,401,403
```

### ▸ O que procurar durante a enumeração?

```text
[+] painéis de administração
[+] arquivos de backup esquecidos
[+] diretórios de desenvolvimento
[+] configs expostos
[+] endpoints de API
[+] login / reset de senha
[+] respostas HTTP inesperadas
```

> `403 Forbidden` também pode revelar que o recurso **existe**, mesmo com acesso negado.

[↑ índice](#indice)

---

# 07 — DNS & REDE

```text
[ MODULE 07 ] NAME RESOLUTION & PROTOCOLS
[ TAGS      ] dig · whois · TCP · UDP · TLS
```

### ▸ Diagnóstico DNS

```bash
# Consulta registros DNS de um domínio
dig example.com

# Consulta apenas o endereço IPv4
dig example.com A

# Consulta registros MX
dig example.com MX

# Consulta registros NS
dig example.com NS

# Consulta usando um resolvedor DNS específico
dig @1.1.1.1 example.com

# Consulta informações de um domínio
whois example.com
```

### ▸ Conceitos de rede

| Protocolo | Função |
|---|---|
| **DNS** | traduz nomes em informações de rede |
| **TCP** | orientado à conexão |
| **UDP** | sem handshake de conexão |
| **HTTP/HTTPS** | comunicação web |
| **SSH** | acesso remoto seguro |
| **TLS** | criptografia + autenticação do canal |

[↑ índice](#indice)

---

# 08 — HTTP & APIs

```text
[ MODULE 08 ] HTTP CLIENT OPS
[ TAGS      ] curl · headers · status · POST
```

### ▸ cURL

```bash
# Faz uma requisição GET
curl -i http://10.65.170.96/

# Exibe apenas os cabeçalhos HTTP
curl -I http://10.65.170.96/

# Exibe o código HTTP retornado
curl -s -o /dev/null -w "%{http_code}\n" \
  http://10.65.170.96/

# Salva a resposta em um arquivo
curl -o pagina.html http://10.65.170.96/

# Segue redirecionamentos HTTP
curl -L http://10.65.170.96/

# Envia dados de formulário para um endpoint autorizado
curl -X POST \
  -d "nome=teste&mensagem=hello" \
  http://127.0.0.1:8080/contact
```

### ▸ Códigos HTTP comuns

| Código | Significado |
|---|---|
| `200` | OK |
| `301` / `302` | redirecionamento |
| `400` | requisição inválida |
| `401` | autenticação necessária / inválida |
| `403` | acesso proibido |
| `404` | não encontrado |
| `429` | rate limit |
| `500` | erro interno do servidor |

[↑ índice](#indice)

---

# 09 — NETCAT & CONEXÕES

```text
[ MODULE 09 ] RAW TCP SOCKETS
[ TAGS      ] nc · listener · probe · lsof
```

### ▸ Listener TCP no macOS

```bash
# Abre um listener TCP na porta 7000
nc -n -l -p 7000

# Abre um listener TCP na porta 4444
nc -n -l -p 4444

# Testa uma conexão TCP com um serviço autorizado
nc -vz 10.65.170.96 80

# Testa uma conexão TCP em outra porta
nc -vz 10.65.170.96 22
```

A sintaxe pode variar conforme a implementação do Netcat. No macOS, `nc -n -l -p 7000` é uma forma compatível com a versão utilizada neste lab.

### ▸ Teste local de conectividade

**Terminal 1:**

```bash
# Aguarda uma conexão local
nc -n -l -p 7000
```

**Terminal 2:**

```bash
# Conecta ao listener no próprio Mac
nc -nv 127.0.0.1 7000
```

Digite uma mensagem no segundo terminal e observe se ela aparece no primeiro. Esse teste verifica o listener sem depender da VPN.

### ▸ Diagnóstico

```bash
# Confirma se a porta está em escuta
lsof -nP -iTCP:7000

# Confere o processo associado ao listener
ps aux | grep '[n]c'

# Verifica as rotas até o alvo
route -n get 10.65.170.96
```

[↑ índice](#indice)

---

# 10 — HYDRA & AUDITORIA DE SENHAS

```text
[ MODULE 10 ] AUTH STRENGTH TESTING
[ TAGS      ] -l · -L · -p · -P · -t · -vV · -o
```

> O Hydra automatiza tentativas de autenticação. Utilize **apenas** em sistemas próprios ou laboratórios que autorizem expressamente esse tipo de teste.

### ▸ Exemplos em ambientes autorizados

```bash
# Testa uma lista de senhas para um usuário de laboratório via SSH
hydra -l jan -P tools/seclist/rockyou.txt \
  -t 4 -vV 10.65.156.192 ssh
```

```bash
# Testa uma senha conhecida para uma conta de laboratório
hydra -l usuario_teste -p senha_de_teste \
  10.65.156.192 ssh
```

```bash
# Usa uma lista pequena de senhas preparada para um laboratório
hydra -l usuario_teste -P senhas_teste.txt \
  -t 2 -vV 10.65.156.192 ssh
```

```bash
# Salva os resultados em um arquivo
hydra -l jan \
  -P tools/seclist/rockyou.txt \
  -t 4 -vV -o hydra.txt 10.65.156.192 ssh
```

### ▸ Opções importantes

| Opção | Função |
|---|---|
| `-l` | usuário único |
| `-L` | lista de usuários |
| `-p` | senha única |
| `-P` | lista de senhas |
| `-t` | tarefas paralelas |
| `-vV` | verbosidade alta |
| `-o` | salva resultados |

> Tentativas repetidas podem bloquear contas e gerar alertas. Prefira listas pequenas em labs e respeite os limites do ambiente.

[↑ índice](#indice)

---

# 11 — ACESSO REMOTO & AUTENTICAÇÃO

```text
[ MODULE 11 ] IDENTITY → AUTHENTICATION → ACCESS
[ TAGS      ] ssh · ftp · sftp · scp
```

> Quando você encontra credenciais durante um CTF, o próximo passo é identificar **qual serviço aceita essas credenciais**.

---

## ▸ SSH — Secure Shell

Usado para acessar remotamente um terminal Linux.

### Conectar com usuário e senha

```bash
# Conecta ao servidor SSH
ssh usuario@10.10.10.10
```

O terminal solicitará a senha. Digite-a quando solicitado; os caracteres não aparecerão na tela.

```bash
# Conecta usando uma porta SSH personalizada
ssh -p 2222 usuario@10.10.10.10

# Ativa mensagens detalhadas para diagnosticar problemas
ssh -v usuario@10.10.10.10

# Executa um comando remoto sem abrir uma sessão interativa
ssh usuario@10.10.10.10 'whoami'

# Sai da sessão SSH
exit
```

**Exemplo em laboratório:**

```bash
ssh jan@10.65.156.192
```

> Substitua o usuário, o IP e a porta pelos dados do seu laboratório autorizado.

### Erros comuns

| Erro | Possível causa |
|---|---|
| `Permission denied` | usuário, senha ou método de auth incorreto |
| `Connection refused` | SSH indisponível ou porta errada |
| `Connection timed out` | rota, firewall ou host offline |
| `Host key verification failed` | fingerprint do host não confere |

Não ignore avisos de chave SSH sem verificar a identidade do servidor.

---

## ▸ FTP — File Transfer Protocol

FTP é usado para transferir arquivos entre um cliente e um servidor.

### Conectar ao servidor

```bash
# Abre uma conexão FTP
ftp 10.10.10.10
```

Quando solicitado, informe o usuário e a senha.

### Comandos dentro do FTP

```text
pwd                 # diretório remoto atual
ls                  # lista remota
cd public           # muda diretório remoto
get documento.txt   # download
put arquivo.txt     # upload
bye                 # encerra sessão
```

> `put` só funciona se a conta tiver permissão de escrita no destino.

### Verificar a porta FTP

```bash
# Verifica se a porta TCP 21 está acessível
nc -vz 10.10.10.10 21

# Verifica se a porta TCP 22 está acessível
nc -vz 10.10.10.10 22
```

A porta 21 é a porta de controle FTP convencional. A transferência de dados pode usar conexões adicionais.

### Segurança do FTP

FTP tradicional transmite credenciais **sem criptografia**. Em redes não confiáveis, prefira SFTP ou FTPS.

---

## ▸ SFTP / SCP — Transferência segura

SFTP utiliza o SSH para transferir arquivos de forma protegida.

```bash
# Abre uma sessão SFTP
sftp usuario@10.10.10.10
```

Dentro da sessão:

```text
ls                  # lista remota
pwd                 # diretório remoto
lls                 # lista local
get documento.txt   # download
put relatorio.txt   # upload
exit                # sai
```

```bash
# Baixa um arquivo remoto para o diretório local atual
scp usuario@10.10.10.10:/home/usuario/relatorio.txt .

# Envia um arquivo para o diretório pessoal do usuário
scp relatorio.txt usuario@10.10.10.10:/home/usuario/

# Exemplo de lab: baixar artefato
scp jan@10.65.156.192:/home/kay/pass.bak \
  rooms/basic-pentest/
```

[↑ índice](#indice)

---

# 12 — IDENTIFICAR SERVIÇOS ANTES DE ENTRAR

```text
[ MODULE 12 ] CREDENTIAL → SERVICE MATCHING
[ TAGS      ] nmap -sV · ports 21/22/80
```

Se você encontrou credenciais, mas não sabe em qual serviço utilizá-las, primeiro identifique as portas abertas.

```bash
# Varre as portas TCP mais comuns
nmap 10.10.10.10

# Identifica versões dos serviços
nmap -sV 10.10.10.10

# Examina serviços nas portas SSH, FTP e HTTP
nmap -sV -p 21,22,80 10.10.10.10

# Salva os resultados para consulta posterior
nmap -sV -oN servicos.txt 10.10.10.10
```

Use esses comandos somente contra alvos próprios ou incluídos no escopo autorizado do seu laboratório.

> **Regra de ouro:** encontrar uma senha **não** significa que ela funciona em todos os serviços. SSH, FTP e apps web podem ter auth diferente.

[↑ índice](#indice)

---

# 13 — ESTEGANOGRAFIA

```text
[ MODULE 13 ] HIDDEN DATA IN MEDIA
[ TAGS      ] stegify · file · exiftool · strings
```

Esteganografia é a técnica de ocultar informações dentro de outro arquivo, como uma imagem.

### ▸ Stegify

Os exemplos abaixo pressupõem que o comando `stegify` esteja instalado e disponível no terminal.

```bash
# Cria um arquivo de texto de teste
echo "mensagem secreta do laboratório" > secreto.txt

# Oculta o arquivo dentro de uma imagem
stegify encode \
  --carrier foto.png \
  --data secreto.txt \
  --result foto_stego.png

# Extrai o conteúdo oculto
stegify decode \
  --carrier foto_stego.png \
  --result recuperado.txt

# Confere o conteúdo extraído
cat recuperado.txt
```

### ▸ Investigar arquivos suspeitos

```bash
# Identifica o formato real do arquivo
file foto.png

# Calcula o hash SHA-256
shasum -a 256 foto.png

# Inspeciona metadados de imagens
exiftool foto.png

# Verifica se há strings legíveis no arquivo
strings foto.png | head -n 50
```

Esses métodos podem revelar metadados ou conteúdo textual, mas não garantem a detecção de todos os dados ocultos.

[↑ índice](#indice)

---

# 14 — ANÁLISE DE ARQUIVOS

```text
[ MODULE 14 ] FILE TRIAGE
[ TAGS      ] file · strings · xxd · diff · md5 · sha256
```

### ▸ Identificação e integridade

```bash
# Identifica o formato de um arquivo
file documento.bin

# Calcula SHA-256
shasum -a 256 documento.bin

# Calcula MD5 para comparação com referências antigas
md5 documento.bin

# Exibe strings ASCII legíveis
strings documento.bin

# Exibe strings com pelo menos 8 caracteres
strings -n 8 documento.bin

# Compara dois arquivos
diff arquivo_original.txt arquivo_modificado.txt

# Exibe os primeiros bytes em hexadecimal
xxd -l 128 documento.bin
```

### ▸ Por que hashes são importantes?

```text
[+] SHA-256 → recomendado para integridade
[+] MD5     → ainda aparece em tools antigas; fraco contra colisões
[+] Hash ≠ criptografia do conteúdo
```

[↑ índice](#indice)

---

# 15 — LOGS & INVESTIGAÇÃO

```text
[ MODULE 15 ] LOG TRIAGE & TIMELINE
[ TAGS      ] grep · tail -f · find -mtime
```

### ▸ Inspeção de logs

```bash
# Procura erros em um log
grep -in "error" aplicacao.log

# Procura falhas de autenticação
grep -in "failed" autenticacao.log

# Conta ocorrências de uma palavra
grep -ic "warning" aplicacao.log

# Acompanha novas linhas em tempo real
tail -f aplicacao.log

# Lista arquivos modificados nas últimas 24 horas
find . -type f -mtime -1
```

Os caminhos e formatos dos logs variam conforme o SO e a aplicação. Alguns arquivos exigem permissões administrativas.

### ▸ Perguntas para uma investigação

```text
[?] Qual processo abriu a conexão?
[?] Qual usuário executou o processo?
[?] Qual IP iniciou a conexão?
[?] Qual serviço escutava a porta?
[?] Há falhas de autenticação nos logs?
[?] O arquivo foi modificado recentemente?
[?] Existe um hash confiável para comparação?
```

[↑ índice](#indice)

---

# 16 — FLUXO DE TRABALHO NO CTF

```text
[ MODULE 16 ] CTF PLAYBOOK
[ TAGS      ] recon → enum → auth → loot → notes
```

```text
[ 1 ] Encontrar o endereço IP
          |
          v
[ 2 ] Identificar portas e serviços
          |
          v
[ 3 ] Descobrir ou receber credenciais
          |
          v
[ 4 ] Identificar qual serviço aceita essas credenciais
          |
          v
[ 5 ] Autenticar-se pelo serviço apropriado
          |
          v
[ 6 ] Verificar usuário, permissões e diretório atual
          |
          v
[ 7 ] Documentar os resultados
```

### ▸ Depois de entrar em um terminal Linux

```bash
# Identifica o usuário atual
whoami

# Mostra UID, GID e grupos
id

# Mostra o diretório atual
pwd

# Lista arquivos e permissões
ls -lah

# Exibe o sistema operacional
uname -a

# Mostra os diretórios pessoais
ls -lah /home
```

### ▸ Verificar binários SUID

```bash
# Lista arquivos com bit SUID setado
find / -perm -4000 -type f 2>/dev/null
```

[↑ índice](#indice)

---

# 17 — CHECKLIST DE PENTEST

```text
[ MODULE 17 ] ENGAGEMENT CHECKLIST
[ TAGS      ] recon · enum · analysis · docs
```

## Fase 1 — Reconhecimento

- [ ] Identificar o escopo autorizado
- [ ] Confirmar o IP da máquina-alvo
- [ ] Verificar conectividade
- [ ] Mapear portas e serviços
- [ ] Registrar os resultados do Nmap

## Fase 2 — Enumeração

- [ ] Identificar tecnologias web
- [ ] Enumerar diretórios e arquivos
- [ ] Examinar respostas HTTP
- [ ] Identificar formulários e endpoints
- [ ] Registrar erros e comportamentos inesperados

## Fase 3 — Análise de segurança

- [ ] Verificar permissões e controles de acesso
- [ ] Investigar possíveis falhas de autenticação
- [ ] Examinar validação / filtragem de entradas
- [ ] Avaliar exposição de arquivos e metadados
- [ ] Registrar evidências reproduzíveis

## Fase 4 — Documentação

- [ ] Salvar saídas das ferramentas
- [ ] Registrar data e hora dos testes
- [ ] Descrever impacto e condições de reprodução
- [ ] Propor correções
- [ ] Confirmar que não houve alteração indevida no ambiente

[↑ índice](#indice)

---

# 18 — GOOGLE BUG HUNTERS (ANDROID / MOBILE)

```text
[ MODULE 18 ] MOBILE VRP RESEARCH
[ TAGS      ] Android · JADX · ADB · Burp · Studio
```

### ▸ Programa & escopo

- Regras / escopo: [Google Mobile Vulnerability Reward Program](https://bughunters.google.com/about/rules/android-friends/google-mobile-vulnerability-reward-program-rules#program-scope)

### ▸ Escopo típico (entidades / marcas relacionadas)

```text
[+] Google LLC
[+] Developed with Google
[+] Research at Google
[+] Red Hot Labs
[+] Google Samples
[+] Fitbit LLC
[+] Nest Labs Inc.
[+] Waymo LLC
[+] Waze
```

> Sempre confirme o escopo **oficial e atual** na página do programa antes de testar qualquer app.

### ▸ O que procurar nesses aplicativos?

O programa prioriza vulnerabilidades com impacto real:

| Classe | Impacto |
|---|---|
| **ACE** (Arbitrary Code Execution) | execução de código não autorizado no contexto do app |
| **Sensitive Data Theft** | credenciais, tokens, mensagens, fotos privadas, etc. |
| **Path Traversal** | escrita arbitrária de arquivos |
| **Intent Redirection** | redirecionamento indevido entre componentes Android |
| **PendingIntent** | uso inseguro de permissões / intenções pendentes |

### ▸ Ferramentas necessárias

| Tool | Função |
|---|---|
| **Android Studio** | IDE + emulador Android |
| **JADX** | análise estática de APK |
| **Burp Suite Community** | interceptação / análise de tráfego HTTP |
| **ADB** | comunicação com o emulador / device |

[↑ índice](#indice)

---

# 19 — ORGANIZAÇÃO DO LABORATÓRIO

```text
[ MODULE 19 ] WORKSPACE LAYOUT
```

```text
tryhackme/
├── security-lab.md          ← este arsenal
├── docs/
├── linux/
├── rooms/                   ← notas / loot por room
│   ├── basic-pentest/
│   ├── chill-hack/
│   └── ...
├── tools/
│   └── seclist/
│       ├── common.txt
│       └── rockyou.txt
└── vpn/                     ← configs .ovpn (não versionar segredos)
```

Estrutura sugerida se quiser separar scans e evidências:

```text
security-lab/
├── README.md
├── wordlists/
│   ├── common.txt
│   └── senhas_teste.txt
├── scans/
│   ├── nmap/
│   └── gobuster/
├── evidence/
│   ├── screenshots/
│   └── logs/
├── samples/
│   ├── images/
│   └── documents/
└── reports/
```

[↑ índice](#indice)

---

# 20 — REGRAS DE SEGURANÇA

```text
[ MODULE 20 ] RULES OF ENGAGEMENT
```

```text
[ 1 ] Teste somente sistemas próprios ou com autorização explícita
[ 2 ] Confirme IP e escopo antes de qualquer scan
[ 3 ] Não compartilhe wordlists com senhas reais / dados pessoais
[ 4 ] Não execute arquivos desconhecidos sem analisá-los
[ 5 ] Faça backup antes de comandos destrutivos
[ 6 ] Não publique credenciais, tokens, chaves ou .ovpn
[ 7 ] Documente resultados e preserve a disponibilidade do ambiente
```

---

```text
╔══════════════════════════════════════════════════════════╗
║                   END OF FILE                            ║
║                                                          ║
║  [+] Knowledge is the first tool.                        ║
║  [+] Enumeration before exploitation.                    ║
║  [+] Evidence before conclusions.                        ║
║  [+] Authorization before testing.                       ║
║                                                          ║
║  STATUS: KEEP LEARNING.                                  ║
╚══════════════════════════════════════════════════════════╝
```

[↑ topo / índice](#indice)
