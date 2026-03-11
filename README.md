# DP Systems - Painel de Acesso

Sistema de login e registro de usuários desenvolvido em Python com interface gráfica Tkinter e banco de dados SQLite.

## Estrutura do Projeto

```
penlab/
├── index.py          # Interface gráfica (Tkinter) - login e registro
├── DataBaser.py      # Conexão e schema do banco SQLite
├── UserData.db       # Banco de dados (criado na primeira execução)
└── icons/            # Ícones da aplicação (logo.png, logoIcon.ico)
```

## Requisitos

- Python 3.x
- Módulos: `tkinter` (incluído no Python), `sqlite3` (incluído no Python)

## Como Executar

```bash
python index.py
```

## Funcionalidades

- **Login** – Autenticação por usuário e senha
- **Registro** – Cadastro de novos usuários (nome, email, usuário, senha)
- **Interface** – Janela gráfica com campos de entrada e botões

## Estrutura do Banco de Dados

| Coluna   | Tipo    | Descrição          |
|----------|---------|--------------------|
| Id       | INTEGER | Chave primária     |
| Name     | TEXT    | Nome do usuário    |
| Email    | TEXT    | E-mail             |
| User     | TEXT    | Nome de login      |
| Password | TEXT    | Senha              |

---

## Vulnerabilidades e Referências

Este software foi desenvolvido com fins educacionais e apresenta vulnerabilidades intencionais. 

### 1. Senhas em Texto Plano

| Campo        | Valor                         |
|--------------|-------------------------------|
| **Severidade** | Crítica                      |
| **Local**      | `DataBaser.py` – tabela Users |
| **Descrição**  | Senhas armazenadas sem criptografia. Qualquer acesso ao arquivo `UserData.db` expõe todas as credenciais. |
| **OWASP Top 10** | [A02:2021 – Cryptographic Failures](https://owasp.org/Top10/A02_2021-Cryptographic_Failures/) |
| **CWE**         | [CWE-256: Plaintext Storage of a Password](https://cwe.mitre.org/data/definitions/256.html) |

### 2. Validação de Registro Inadequada

| Campo        | Valor                                      |
|--------------|--------------------------------------------|
| **Severidade** | Alta                                     |
| **Local**      | `index.py` linha 119                      |
| **Descrição**  | Bug na condição: `if (Name == "" and Email, "" and User == "" and Pass == ""):` — uso de vírgula em vez de `==` para `Email` gera tupla e a validação não funciona. Permite registro com campos vazios. |
| **OWASP Top 10** | [A01:2021 – Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/) |
| **CWE**         | [CWE-20: Improper Input Validation](https://cwe.mitre.org/data/definitions/20.html) |

### 3. Ausência de Proteção Contra Força Bruta

| Campo        | Valor                                      |
|--------------|--------------------------------------------|
| **Severidade** | Média                                   |
| **Local**      | `index.py` – função `Login()`            |
| **Descrição**  | Sem rate limiting, bloqueio de conta ou CAPTCHA. Facilita ataques de força bruta e enumeração de usuários. |
| **OWASP Top 10** | [A07:2021 – Identification and Authentication Failures](https://owasp.org/Top10/A07_2021-Identification_and_Authentication_Failures/) |
| **CWE**         | [CWE-307: Improper Restriction of Excessive Authentication Attempts](https://cwe.mitre.org/data/definitions/307.html) |

### 4. Ausência de Validação de Entrada

| Campo        | Valor                                      |
|--------------|--------------------------------------------|
| **Severidade** | Média                                   |
| **Local**      | `index.py` – campos de entrada           |
| **Descrição**  | Nenhuma validação de formato (ex.: email), tamanho ou caracteres. Prepared statements protegem contra SQL Injection, mas não contra dados inválidos ou maliciosos. |
| **OWASP Top 10** | [A03:2021 – Injection](https://owasp.org/Top10/A03_2021-Injection/) |
| **CWE**         | [CWE-20: Improper Input Validation](https://cwe.mitre.org/data/definitions/20.html) |

### 5. Mensagens de Erro e Tratamento de Exceções

| Campo        | Valor                                      |
|--------------|--------------------------------------------|
| **Severidade** | Baixa                                   |
| **Local**      | `index.py` – função `Login()` linhas 72–76 |
| **Descrição**  | `except:` genérico mascara erros. Mensagens distintas (“Acess allowed” vs “Acess denied”) podem ajudar enumeração de usuários. |
| **OWASP Top 10** | [A05:2021 – Security Misconfiguration](https://owasp.org/Top10/A05_2021-Security_Misconfiguration/) |
| **CWE**         | [CWE-209: Information Exposure Through Error Message](https://cwe.mitre.org/data/definitions/209.html) |

---

### Referências Gerais

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [CWE Top 25 Most Dangerous Software Weaknesses](https://cwe.mitre.org/top25/)
- [NIST Secure Software Development Framework](https://csrc.nist.gov/projects/ssdf)

---

*Projeto para fins educacionais – laboratório PenLab*
