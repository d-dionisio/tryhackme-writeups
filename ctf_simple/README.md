# 🛡️ TryHackMe – CTF Simples Write-up

<p align="center">
  <img src="https://tryhackme-images.s3.amazonaws.com/room-icons/linux.png" width="120"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/TryHackMe-CTF%20Simples-red?style=for-the-badge&logo=tryhackme">
  <img src="https://img.shields.io/badge/Difficulty-Easy-green?style=for-the-badge">
  <img src="https://img.shields.io/badge/Status-Completed-success?style=for-the-badge">
  <img src="https://img.shields.io/badge/Focus-Web%20%2B%20PrivEsc-blue?style=for-the-badge">
</p>

---

## 📌 Visão Geral

Este laboratório aborda um cenário básico de **pentest web**, incluindo:

* Enumeração de serviços
* Identificação de vulnerabilidades web
* Exploração de SQL Injection
* Acesso remoto via SSH
* Escalação de privilégios

> ⚠️ **Nota:** Credenciais, flags e dados sensíveis não serão divulgados neste write-up para manter o caráter prático do laboratório.

---

## 🧭 Attack Path

```text
Nmap Scan
   ↓
Identificação de serviço web
   ↓
Análise de parâmetros
   ↓
SQL Injection (Time-based Blind)
   ↓
Extração de credenciais
   ↓
Acesso SSH
   ↓
Enumeração local
   ↓
Privilege Escalation (binário abusável)
   ↓
Root
```

---

## 🔍 Enumeração Inicial

Scan com Nmap:

```bash
nmap -Pn -p 1-1000 <IP>
```

### 📊 Portas abertas:

* Identifique os serviços disponíveis
* Observe especialmente portas comuns como HTTP e SSH

---

## 🌐 Análise Web

A aplicação web está disponível em:

```
http://<IP>/simple/
```

### 🔎 Observações:

* Parâmetros dinâmicos presentes na URL
* Possível interação com banco de dados
* Indícios de CMS vulnerável

---

## 💣 Identificação da Vulnerabilidade

Teste realizado com `curl`:

```bash
curl -s "http://<IP>/simple/?mact=News,m1_,default,0&m1_idlist=1) AND SLEEP(5)-- -"
```

### 🧠 Resultado:

* Atraso na resposta indica execução no banco
* Vulnerabilidade identificada como:

👉 **SQL Injection (time-based blind)**
👉 Associada à **CVE-2019-9053**

---

## 🔐 Extração de Credenciais

Através da exploração da vulnerabilidade:

* Foi possível obter credenciais válidas
* Processo realizado via inferência (blind SQLi)

> ⚠️ Credenciais não serão divulgadas

---

## 🚪 Acesso Inicial

Com as credenciais obtidas:

```bash
ssh <user>@<IP>
```

---

## 🧾 Flag de Usuário

* Localizada após acesso inicial ao sistema

> ⚠️ Flag omitida propositalmente

---

## 👥 Enumeração de Usuários

Durante a análise do sistema:

* Foi identificado outro usuário no diretório `/home`

---

## 🔼 Escalação de Privilégios

Durante a enumeração:

* Identificação de binário com permissões inadequadas
* Possibilidade de abuso para execução de comandos

Ferramenta utilizada:

* **vim** (abuso de binário) - sudo vim -c ':!/bin/sh'

---

## 👑 Flag Root

* Obtida após elevação de privilégios

> ⚠️ Flag não divulgada

---

## 🧠 Conclusão

Este laboratório demonstra:

* Como identificar e explorar SQL Injection
* Importância da análise de parâmetros web
* Uso de credenciais para acesso lateral
* Técnicas básicas de privilege escalation

---

## 🛠️ Ferramentas Utilizadas

* Nmap
* curl
* SSH
* vim

---

## 📚 Lições Aprendidas

* Enumeração é essencial
* SQL Injection ainda é extremamente comum
* Pequenas falhas podem comprometer todo o sistema
* Conhecimento de binários do sistema é crucial para priv esc

---

## 👨‍💻 Autor

**Douglas Dionísio**
💻 Estudante de Segurança da Informação
🚀 Foco em Pentest & Cybersecurity
