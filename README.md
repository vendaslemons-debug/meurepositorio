# 🛡️ Simulação de Ataques de Força Bruta com Medusa no Kali Linux

## 📝 Descrição do Projeto
Este projeto foi desenvolvido como parte de um desafio prático na **DIO (Digital Innovation One)**. O objetivo principal é consolidar conhecimentos em segurança da informação, testes de penetração e hardening de sistemas, utilizando o **Kali Linux** e a ferramenta **Medusa** para simular ataques de força bruta contra serviços vulneráveis hospedados no **Metasploitable 2** e no **DVWA (Damn Vulnerable Web Application)**.

> ⚠️ **Aviso Legal:** Este projeto foi executado estritamente em ambiente de laboratório controlado, com fins estritamente educacionais e de pesquisa em segurança cibernética.

---

## 💻 Cenário de Laboratório e Configuração

O ambiente foi montado utilizando o **VirtualBox** com duas Máquinas Virtuais (VMs) configuradas em uma rede isolada do tipo **Placa de Rede Exclusiva de Hospedeiro (Host-Only)** para garantir a segurança dos testes.

*   **Atacante:** Kali Linux (`IP: 192.168.56.101`)
*   **Alvo:** Metasploitable 2 / DVWA (`IP: 192.168.56.102`)

---

## 🛠️ Ferramentas Utilizadas
*   **Kali Linux:** Sistema operacional focado em testes de invasão.
*   **Metasploitable 2:** VM intencionalmente vulnerável para treinar técnicas de segurança.
*   **Medusa:** Ferramenta de força bruta modular, rápida e paralela para login em múltiplos protocolos.
*   **DVWA:** Aplicação web em PHP/MySQL altamente vulnerável.

---

## 🚀 Execução dos Cenários de Ataque

### 🛠️ Preparação: Criação de Wordlists Simples
Antes dos ataques, foram estruturadas wordlists personalizadas (disponíveis na pasta `/wordlists` deste repositório) para otimizar os testes.

*   `users.txt`: Contém usuários comuns como `admin`, `user`, `root`, `msfadmin`.
*   `passwords.txt`: Contém senhas fracas comuns como `123456`, `password`, `admin`, `msfadmin`.

---

### 🔥 Cenário 1: Força Bruta no Serviço FTP
O protocolo FTP (porta 21) do Metasploitable 2 foi alvo de uma tentativa de quebra de credenciais em lote.

**Comando utilizado:**
```bash
medusa -h 192.168.56.102 -U wordlists/users.txt -P wordlists/passwords.txt -M ftp
```

**Explicação dos parâmetros:**
*   `-h`: Define o IP do host alvo.
*   `-U`: Caminho para a lista de usuários.
*   `-P`: Caminho para a lista de senhas.
*   `-M ftp`: Define o módulo do protocolo (FTP).

**Resultado esperado:**
O Medusa identificou com sucesso a credencial válida: `msfadmin:msfadmin`.

---

### 🔥 Cenário 2: Automação em Formulário Web (DVWA - HTTP-Form)
Simulação de ataque contra a tela de login do DVWA configurado no nível de segurança *Low*.

**Comando utilizado:**
```bash
medusa -h 192.168.56.102 -u admin -P wordlists/passwords.txt -M http -m FORM:dvwa/login.php -F
```

**Explicação dos parâmetros adicionais:**
*   `-u admin`: Ataque direcionado a um único usuário conhecido.
*   `-m FORM:...`: Especifica o caminho do formulário web.
*   `-F`: Interrompe a execução assim que encontrar a primeira combinação correta.

---

### 🔥 Cenário 3: Password Spraying e Enumeração em SMB
Ataque focado no serviço de compartilhamento de arquivos SMB (porta 445), testando uma única senha contra múltiplos usuários para evitar o bloqueio de contas (*account lockout*).

**Comando utilizado:**
```bash
medusa -h 192.168.56.102 -U wordlists/users.txt -p "msfadmin" -M smbnt
```

---

## 🛡️ Medidas de Mitigação e Prevenção (Hardening)

Para proteger sistemas contra os ataques demonstrados acima, recomendam-se as seguintes boas práticas de segurança:

1.  **Políticas de Senhas Fortes:** Implementar a obrigatoriedade de senhas complexas (letras maiúsculas, minúsculas, números e caracteres especiais) e tamanho mínimo de 12 caracteres.
2.  **Mecanismos de Bloqueio de Conta (Lockout Policy):** Bloquear temporariamente contas que errem a senha de 3 a 5 vezes consecutivas, mitigando a eficácia do brute force clássico.
3.  **Implementação de Segundo Fator de Autenticação (MFA):** Essencial para serviços críticos e aplicações web, garantindo proteção mesmo se a senha for vazada.
4.  **Uso de CAPTCHAs:** Adicionar CAPTCHAs em formulários de login web para impedir a automação por ferramentas como o Medusa.
5.  **Monitoramento e Rate Limiting:** Utilizar ferramentas como o **Fail2ban** para monitorar logs de autenticação (como SSH/FTP) e banir IPs que apresentem comportamento suspeito ou excesso de requisições.

---

## 🧠 Conclusão e Aprendizados
A realização deste desafio permitiu compreender na prática como funcionam os ataques de adivinhação de credenciais em diferentes camadas (rede e aplicação). Ficou evidente a velocidade com que ferramentas automatizadas conseguem comprometer contas que utilizam credenciais fracas ou padrões de fábrica, reforçando a importância vital de práticas de *hardening* e conscientização corporativa sobre segurança de senhas.

---
🛠️ Desenvolvido por [Seu Nome](https://github.com) durante o Bootcamp na DIO!# meurepositorio
