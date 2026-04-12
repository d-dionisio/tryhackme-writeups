# 🛡️ TryHackMe – Kenobi Write-up

<p align="center">
  <img src="https://tryhackme-images.s3.amazonaws.com/room-icons/kenobi.png" width="120"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/TryHackMe-Kenobi-red?style=for-the-badge&logo=tryhackme">
  <img src="https://img.shields.io/badge/Difficulty-Easy-green?style=for-the-badge">
  <img src="https://img.shields.io/badge/Status-Completed-success?style=for-the-badge">
  <img src="https://img.shields.io/badge/Focus-Pentesting-blue?style=for-the-badge">
</p>

---

## 📌 Visão Geral

Este laboratório aborda um cenário prático de **teste de invasão**, passando por etapas essenciais como:

* Enumeração de serviços
* Descoberta de diretórios ocultos
* Exploração de SMB
* Quebra de chave SSH
* Acesso ao sistema

> ⚠️ **Nota:** Por boas práticas e para incentivar o aprendizado, credenciais (usuários e senhas) descobertas durante este laboratório não serão divulgadas neste write-up, permitindo que outros pratiquem e realizem suas próprias descobertas.

---

## 🧭 Attack Path

```text id="j3x8k1"
Nmap Scan
   ↓
Web Enumeration (Gobuster)
   ↓
/development → vazamento de informações
   ↓
Enumeração SMB (Anonymous Share)
   ↓
Descoberta de usuários
   ↓
Acesso a arquivos sensíveis
   ↓
Extração de chave privada
   ↓
Conversão com ssh2john
   ↓
Quebra com John the Ripper
   ↓
Acesso SSH
```

---

## 🔍 Enumeração Inicial

Scan com Nmap:

```bash id="j4m9z2"
nmap -sC -sV <IP>
```

### 📊 Portas abertas:

* 22 → SSH
* 80 → HTTP
* 139 / 445 → SMB
* 8009 → AJP13
* 8080 → Tomcat

---

## 🌐 Enumeração Web

Com Gobuster:

```bash id="2d6xq9"
gobuster dir -u http://<IP> -w /usr/share/wordlists/dirb/common.txt
```

### 🔎 Descoberta:

* `/development`

Arquivos encontrados:

* Arquivos de texto contendo informações internas do sistema

### 🧠 Insights:

* Referência a tecnologias utilizadas no servidor
* Indícios de configurações inseguras
* Possível uso de credenciais fracas

---

## 📡 Enumeração SMB

```bash id="n7z2t5"
smbclient -L //<IP> -N
```

### 📁 Share disponível:

* Anonymous

```bash id="v8y4pq"
smbclient //<IP>/Anonymous -N
```

Conteúdo identificado:

* Arquivos internos com informações administrativas

---

## 👥 Enumeração de Usuários

```bash id="h5s9wr"
enum4linux <IP>
```

### 🔎 Resultado:

Foram identificados usuários válidos no sistema.

> ⚠️ Os nomes dos usuários não serão divulgados para manter a proposta prática do laboratório.

---

## 🔐 Obtenção de Acesso

Durante a exploração, foi possível identificar e acessar uma **chave privada SSH** presente no sistema.

---

## 🔓 Quebra da Chave SSH

Conversão:

```bash id="c3p7yu"
python3 ssh2john.py kay_id_rsa > kay_hash.txt
```

Execução com John the Ripper:

```bash id="x9t2ev"
john kay_hash.txt --wordlist=/usr/share/wordlists/rockyou.txt
```

### 🔑 Resultado:

A passphrase da chave foi descoberta com sucesso por meio de ataque de dicionário.

> ⚠️ A senha não será divulgada neste material.

---

## 🚪 Acesso via SSH

```bash id="k1m7ru"
chmod 600 kay_id_rsa
ssh -i kay_id_rsa <user>@<IP>
```

---

## 🧠 Conclusão

Este laboratório reforça:

* A importância da enumeração detalhada
* Como pequenas falhas podem levar à exploração completa
* Eficiência de ataques offline
* Uso estratégico de wordlists
* Segurança de credenciais e chaves privadas

---

## 🛠️ Ferramentas Utilizadas

* Nmap
* Gobuster
* John the Ripper
* smbclient
* enum4linux

---

## 📚 Lições Aprendidas

* Enumeração é a etapa mais crítica
* Informações expostas podem ser exploradas facilmente
* Ataques inteligentes são mais eficientes que força bruta
* Boas práticas de segurança evitam comprometimentos

---

## 👨‍💻 Autor

**Douglas Dionísio**
💻 Estudante de Segurança da Informação
🚀 Foco em Pentest & Cybersecurity
