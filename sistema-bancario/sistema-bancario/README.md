# 🏦 Sistema Bancário Digital (CLI) — Python + POO

Aplicação de linha de comando (CLI) que simula um banco digital: cadastro de clientes, criação de contas (corrente e poupança), depósitos, saques e extrato. Projeto com foco em **Programação Orientada a Objetos** e organização em módulos.

---

## 📑 Índice

1. [Funcionalidades](#-funcionalidades)
2. [Requisitos](#-requisitos)
3. [Como baixar o projeto](#-como-baixar-o-projeto)
4. [Estrutura de pastas](#-estrutura-de-pastas)
5. [Como executar](#-como-executar)
6. [Guia de uso passo a passo](#-guia-de-uso-passo-a-passo)
7. [Regras de negócio](#-regras-de-negócio)
8. [Conceitos de POO demonstrados](#-conceitos-de-poo-demonstrados)
9. [Solução de problemas](#-solução-de-problemas)
10. [Limitações conhecidas](#-limitações-conhecidas)
11. [Ideias de melhorias](#-ideias-de-melhorias)

---

## ✨ Funcionalidades

- **Adicionar cliente** (nome + CPF; CPFs duplicados são recusados).
- **Criar conta** corrente ou poupança, vinculada a um cliente já cadastrado.
- **Acessar conta** pelo número e então:
  - Depositar
  - Sacar
  - Ver extrato com histórico de transações (data e hora)
- Tratamento de erros com **exceções personalizadas** (saldo insuficiente e conta inexistente).

---

## 📋 Requisitos

| Item | Detalhe |
|------|---------|
| Python | **3.8 ou superior** (testado com 3.14) |
| Bibliotecas externas | **Nenhuma** — usa apenas a biblioteca padrão (`abc`, `datetime`) |
| Sistema operacional | Windows, macOS ou Linux |
| Terminal | Prompt de Comando, PowerShell, Terminal (macOS/Linux) |

Para verificar se o Python está instalado:

```bash
python --version
```

> Em alguns sistemas (macOS/Linux) o comando é `python3 --version`. Se o comando não for reconhecido, baixe o Python em <https://www.python.org/downloads/> e, no Windows, marque **"Add Python to PATH"** durante a instalação.

---

## 📥 Como baixar o projeto

### Opção A — Com Git (recomendado)

```bash
git clone <URL-DO-SEU-REPOSITORIO>
cd sistema-bancario
```

> Substitua `<URL-DO-SEU-REPOSITORIO>` pelo endereço do repositório onde o projeto está hospedado (ex.: GitHub).

### Opção B — Download em ZIP

1. Na página do repositório, clique em **Code → Download ZIP**.
2. Extraia o arquivo ZIP.
3. Abra o terminal dentro da pasta extraída (`sistema-bancario`).

### Opção C — Montagem manual a partir dos arquivos soltos

Se você recebeu os arquivos `.py` todos juntos, em uma mesma pasta, crie a estrutura abaixo e mova cada arquivo para o lugar correto (os `import` do código dependem dessa organização):

```text
sistema-bancario/
├── entidades/
│   ├── __init__.py        (arquivo vazio)
│   ├── cliente.py
│   └── conta.py
├── operacoes/
│   ├── __init__.py        (arquivo vazio)
│   └── banco.py
├── utilitarios/
│   ├── __init__.py        (arquivo vazio)
│   └── exceptions.py
└── main.py
```

Comandos para criar as pastas e os `__init__.py`:

**Linux / macOS**
```bash
mkdir -p sistema-bancario/entidades sistema-bancario/operacoes sistema-bancario/utilitarios
touch sistema-bancario/entidades/__init__.py \
      sistema-bancario/operacoes/__init__.py \
      sistema-bancario/utilitarios/__init__.py
```

**Windows (PowerShell)**
```powershell
mkdir sistema-bancario\entidades, sistema-bancario\operacoes, sistema-bancario\utilitarios
New-Item sistema-bancario\entidades\__init__.py, sistema-bancario\operacoes\__init__.py, sistema-bancario\utilitarios\__init__.py
```

Depois mova os arquivos:

| Arquivo | Destino |
|---------|---------|
| `cliente.py` | `entidades/` |
| `conta.py` | `entidades/` |
| `banco.py` | `operacoes/` |
| `exceptions.py` | `utilitarios/` |
| `main.py` | raiz (`sistema-bancario/`) |

> Arquivos `.pyc` e pastas `__pycache__` são gerados automaticamente pelo Python e **não** são necessários — podem ser ignorados ou apagados.

---

## 🗂 Estrutura de pastas

| Caminho | Responsabilidade |
|---------|------------------|
| `entidades/` | Entidades de dados: `Cliente` e as contas (`Conta`, `ContaCorrente`, `ContaPoupanca`) |
| `operacoes/` | Lógica de negócio: classe `Banco`, que gerencia clientes e contas |
| `utilitarios/` | Utilitários: exceções personalizadas (`SaldoInsuficienteError`, `ContaInexistenteError`) |
| `main.py` | Ponto de entrada: menus e interação com o usuário (CLI) |

---

## ▶️ Como executar

1. Abra o terminal e entre na pasta do projeto:

   ```bash
   cd sistema-bancario
   ```

2. Execute a aplicação:

   ```bash
   python main.py
   ```

   (ou `python3 main.py` em macOS/Linux)

> ⚠️ **Importante:** execute o comando **de dentro da pasta raiz** `sistema-bancario`, onde está o arquivo `main.py`. Rodar de outra pasta causa erros de importação.

Ao iniciar, você verá o menu principal:

```text
--- Sistema Bancário Digital ---

1. Adicionar Cliente
2. Criar Conta
3. Acessar Conta
4. Sair

Escolha uma opção:
```

---

## 🧭 Guia de uso passo a passo

O fluxo correto é sempre: **cliente → conta → operações**.

### 1. Cadastrar um cliente

Escolha `1` e informe nome e CPF:

```text
Escolha uma opção: 1
Digite o nome do cliente: Maria Silva
Digite o CPF do cliente: 123.456.789-00
Cliente Maria Silva adicionado com sucesso!
```

### 2. Criar uma conta

Escolha `2`, informe o CPF cadastrado e o tipo (`corrente` ou `poupanca`, sem acento):

```text
Escolha uma opção: 2
Digite o CPF do cliente para vincular a conta: 123.456.789-00
Digite o tipo da conta (corrente/poupanca): corrente
Conta corrente nº 1 criada para o cliente Maria Silva.
```

Anote o **número da conta** exibido — ele será necessário para acessá-la. As contas são numeradas em sequência (1, 2, 3...).

> O CPF deve ser digitado **exatamente igual** ao cadastrado (mesma pontuação).

### 3. Acessar a conta

Escolha `3` e digite o número da conta:

```text
Escolha uma opção: 3
Digite o número da conta: 1

--- Operações para Conta Nº 1 ---
Cliente: Maria Silva | Saldo: R$0.00
1. Depositar
2. Sacar
3. Ver Extrato
4. Voltar ao Menu Principal
```

**Depositar** (`1`):
```text
Digite o valor para depósito: 1000
Depósito de R$1000.00 realizado com sucesso.
```

**Sacar** (`2`):
```text
Digite o valor para saque: 300
Saque de R$300.00 realizado com sucesso.
```

**Ver extrato** (`3`):
```text
--- Extrato da Conta Nº 1 ---
Cliente: Maria Silva
Saldo atual: R$700.00
Histórico de transações:
- 16/10/2026 14:32:10: Depósito de R$1000.00
- 16/10/2026 14:32:25: Saque de R$300.00
--------------------------------------
```

**Voltar** (`4`): retorna ao menu principal.

> Use **ponto** como separador decimal (ex.: `150.75`), não vírgula.

### 4. Sair

No menu principal, escolha `4`. Você também pode interromper a qualquer momento com `Ctrl + C`.

---

## 📐 Regras de negócio

| Regra | Conta Corrente | Conta Poupança |
|-------|----------------|----------------|
| Saldo inicial | R$ 0,00 | R$ 0,00 |
| Limite de cheque especial | **R$ 500,00** (padrão) | Não possui |
| Saque permitido até | saldo + limite | apenas o saldo |
| Saldo pode ficar negativo? | Sim, até −R$ 500,00 | Não |

Regras gerais:

- Depósitos e saques com valor **menor ou igual a zero** são recusados.
- Saque acima do permitido gera o erro `SaldoInsuficienteError`, exibindo saldo disponível e valor tentado:
  ```text
  Erro na operação: Saldo insuficiente para realizar o saque. Saldo atual: R$700.00, Tentativa de saque: R$5000.00
  ```
- Número de conta inexistente gera `ContaInexistenteError`:
  ```text
  Erro: A conta especificada não foi encontrada. Número da conta: 99
  ```
- Dois clientes não podem ter o mesmo CPF.

---

## 🧠 Conceitos de POO demonstrados

| Conceito | Onde aparece |
|----------|--------------|
| **Classe abstrata** | `Conta(ABC)` com método abstrato `sacar` |
| **Herança** | `ContaCorrente` e `ContaPoupanca` herdam de `Conta` |
| **Polimorfismo** | `sacar()` se comporta de forma diferente em cada tipo de conta |
| **Encapsulamento** | Atributos protegidos (`_saldo`, `_numero`) e `@property saldo` |
| **Atributo/método de classe** | `_total_contas` e `get_total_contas()` |
| **Composição** | `Banco` "tem" clientes e contas; `Cliente` "tem" contas |
| **Exceções customizadas** | `SaldoInsuficienteError`, `ContaInexistenteError` |
| **Modularização** | Separação em pacotes de entidades, operações e utilitários |

---

## 🛠 Solução de problemas

| Problema | Causa provável | Solução |
|----------|----------------|---------|
| `'python' não é reconhecido...` | Python não instalado ou fora do PATH | Instale o Python e/ou use `python3` / `py` |
| `ModuleNotFoundError: No module named 'operacoes'` (ou `entidades`, `utilitarios`) | Pastas fora do lugar, sem `__init__.py`, ou comando executado de outra pasta | Confira a [estrutura de pastas](#-estrutura-de-pastas) e execute de dentro de `sistema-bancario` |
| `ImportError` ao importar `Conta`/`Cliente` | Arquivo na pasta errada | Verifique a tabela de destino dos arquivos |
| `Cliente não encontrado` ao criar conta | CPF digitado diferente do cadastrado | Digite o CPF idêntico ao do cadastro |
| `Tipo de conta inválido` | Digitou `poupança` com acento ou outro texto | Use exatamente `corrente` ou `poupanca` |
| `Erro: Entrada inválida` | Valor não numérico, ou vírgula no decimal | Digite apenas números, com ponto decimal |
| Comportamento estranho após atualizar o código | Cache antigo (`__pycache__`/`.pyc`) | Apague as pastas `__pycache__` e rode novamente |

---

## ⚠️ Limitações conhecidas

- **Sem persistência:** todos os dados ficam apenas em memória. Ao encerrar o programa, clientes, contas e saldos são perdidos.
- **CPF sem validação:** qualquer texto é aceito como CPF.
- **Entrada inválida no menu da conta:** digitar um valor não numérico em depósito/saque exibe o erro e **retorna ao menu principal** (é preciso acessar a conta novamente).
- **Sem autenticação:** qualquer pessoa que souber o número da conta pode acessá-la.
- **Interface apenas em terminal**, sem interface gráfica ou web.

---

## 🚀 Ideias de melhorias

- Persistir dados em arquivo (JSON/CSV) ou banco de dados (SQLite).
- Validar CPF e normalizar a formatação (aceitar com ou sem pontos).
- Adicionar transferência entre contas e rendimento mensal na poupança.
- Criar senha/PIN para acesso às contas.
- Listar clientes e contas cadastrados.
- Escrever testes automatizados com `unittest` ou `pytest`.
- Evoluir para uma API (Flask/FastAPI) ou interface web.
