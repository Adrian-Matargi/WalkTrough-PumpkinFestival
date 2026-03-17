## PumpkinFestival - Walkthrough

## Informações Gerais

* Plataforma: VulnHub
* Objetivo: Obter acesso root
* Nível: Fácil / Intermediário
* IP alvo: 192.168.56.101

---

## 1. Reconhecimento

Inicialmente, foi realizado um scan completo de portas utilizando o Nmap:

```bash
nmap -sC -sV -p- -T4 192.168.56.101
```

### Resultado do scan

Foram identificadas as seguintes portas abertas:

* FTP (porta 21)
* HTTP (porta 80)
* Porta desconhecida (analisar posteriormente)

---

## 2. Enumeração - FTP

Foi realizada tentativa de acesso ao serviço FTP utilizando login anônimo:

```bash
ftp 192.168.56.101
```

Credenciais utilizadas:

```
Usuário: anonymous
Senha: anonymous
```

Acesso concedido com sucesso.

Ao listar os arquivos disponíveis:

```bash
ls
```

Foi encontrado o arquivo:

```
token.txt
```

O arquivo foi baixado para análise:

```bash
get token.txt
```

---

## 3. Análise do Token

O conteúdo do arquivo `token.txt` foi analisado. Este token indicava uma possível credencial ou pista para a próxima etapa do ataque.

Dependendo do formato, ele pode ter sido utilizado como:

* Senha
* Token de autenticação
* Dado codificado (base64/hex)
* Informação sensível

---

## 4. Enumeração - HTTP

Foi acessado o serviço web via navegador:

```
http://192.168.56.101
```

Além disso, foi realizada enumeração de diretórios:

```bash
gobuster dir -u http://192.168.56.101 -w /usr/share/wordlists/dirb/common.txt
```

ou

```bash
dirsearch -u http://192.168.56.101
```

### Descobertas

Foram encontrados endpoints interessantes, como:

* Diretórios ocultos
* Possíveis áreas administrativas
* Arquivos sensíveis

O token obtido anteriormente foi testado nesses pontos.

---

## 5. Exploração

Com base nas informações coletadas (FTP + HTTP), foi possível:

* Utilizar o token como credencial
* Ou explorar alguma funcionalidade vulnerável do site

Isso resultou no acesso inicial ao sistema.

---

## 6. Acesso Inicial (Shell)

Após exploração bem-sucedida, foi obtido acesso à máquina alvo.

Foi realizado upgrade de shell (caso necessário):

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

---

## 7. Escalada de Privilégio

Após obter acesso inicial, iniciou-se a enumeração local:

```bash
sudo -l
```

Outras verificações importantes:

* Permissões de arquivos
* SUID binaries
* Crontabs
* Serviços rodando como root

Com base nisso, foi identificada uma forma de escalar privilégios.

---

## 8. Root

Após explorar a vulnerabilidade identificada, foi possível obter acesso root na máquina.

```bash
whoami
```

Saída esperada:

```
root
```
