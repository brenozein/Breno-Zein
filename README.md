Aqui está o seu **README totalmente estilizado em Markdown**, organizado, padronizado e pronto para ser colado no GitHub.

---

# 📘 Projeto de Controle de Estoque – README

## 🚀 Instalação e Execução do Projeto

### **Pré-requisitos**

* Node.js + npm (preferencialmente instalados via **nvm**)

### **Passo a passo**

```bash
# Clone o repositório
git clone <YOUR_GIT_URL>

# Entre na pasta do projeto
cd <YOUR_PROJECT_NAME>

# Instale as dependências
npm i

# Instale o Vite (versão 4)
npm install vite@v4

# Execute o projeto
npm run dev
```

---

## 🧰 Tecnologias utilizadas

* **Vite**
* **TypeScript**
* **React**
* **shadcn-ui**
* **Tailwind CSS**

---

# 📄 1. Lista de Requisitos Funcionais

### **RF01 – Cadastrar produtos**

O sistema deve permitir o cadastro de produtos contendo nome, categoria, descrição e demais características técnicas.

### **RF02 – Editar produtos**

O sistema deve permitir a atualização dos dados de um produto já cadastrado.

### **RF03 – Excluir produtos**

O sistema deve permitir excluir um produto do estoque.

### **RF04 – Listar produtos**

Usuários autenticados podem visualizar todos os produtos cadastrados.

### **RF05 – Manter controle automático de estoque**

O estoque atual deve ser atualizado automaticamente conforme movimentações de entrada ou saída.

### **RF06 – Registrar movimentações de estoque**

O sistema deve permitir registrar movimentações do tipo entrada ou saída, vinculadas a um produto.

### **RF07 – Registrar autor da movimentação**

Cada movimentação deve conter o ID do usuário responsável.

### **RF08 – Validar quantidade positiva**

O sistema deve aceitar apenas movimentações com quantidade maior que zero.

### **RF09 – Atualizar estoque automaticamente**

Ao registrar uma movimentação:

* Entrada ➝ soma ao estoque
* Saída ➝ subtrai do estoque

### **RF10 – Listar movimentações**

Usuários autenticados podem visualizar todas as movimentações.

### **RF11 – Criar automaticamente perfil de usuário**

Ao criar um usuário na tabela `auth.users`, um perfil é criado automaticamente na tabela `profiles`.

### **RF12 – Editar perfil do usuário**

O usuário pode alterar apenas o próprio perfil.

### **RF13 – Visualizar perfis**

Usuários autenticados podem visualizar todos os perfis.

### **RF14 – Verificar estoque mínimo**

O sistema deve armazenar um estoque mínimo por produto e indicar quando estiver abaixo dele.

### **RF15 – Registrar data de criação e atualização**

Os campos `created_at` e `updated_at` devem ser preenchidos automaticamente.

---

<img width="1159" height="432" src="https://github.com/user-attachments/assets/aa96c0ed-4140-492b-825a-d59a77aec779" />

---

# 📊 2. DER – Diagrama Entidade-Relacionamento

## **ENTIDADE: Products**

Atributos:

* id (PK)
* name
* description
* category
* voltage
* resolution
* dimensions
* storage
* connectivity
* minimum_stock
* current_stock
* unit_price
* created_at
* updated_at

---

## **ENTIDADE: Stock_Movements**

Atributos:

* id (PK)
* product_id (FK → Products.id)
* user_id (FK → auth.users.id)
* movement_type
* quantity
* movement_date
* notes
* created_at

---

## **ENTIDADE: Profiles**

Atributos:

* id (PK & FK → auth.users.id)
* full_name
* role
* created_at
* updated_at

---

## 🔗 **Relacionamentos**

### **1. Products 1 — N Stock_Movements**

Um produto possui várias movimentações.
`stock_movements.product_id → products.id`

### **2. Users 1 — 1 Profiles**

Cada usuário possui exatamente um perfil.
`profiles.id → auth.users.id`

### **3. Users 1 — N Stock_Movements**

Um usuário pode registrar várias movimentações.
`stock_movements.user_id → auth.users.id`

---

## 📄 DER em texto estruturado

```
ENTIDADE: PRODUCTS
  - id (PK)
  - name
  - description
  - category
  - voltage
  - resolution
  - dimensions
  - storage
  - connectivity
  - minimum_stock
  - current_stock
  - unit_price
  - created_at
  - updated_at

ENTIDADE: STOCK_MOVEMENTS
  - id (PK)
  - product_id (FK → PRODUCTS.id)
  - user_id (FK → AUTH.USERS.id)
  - movement_type
  - quantity
  - movement_date
  - notes
  - created_at

ENTIDADE: PROFILES
  - id (PK & FK → AUTH.USERS.id)
  - full_name
  - role
  - created_at
  - updated_at

RELACIONAMENTOS:
  PRODUCTS (1) ---- (N) STOCK_MOVEMENTS
  AUTH.USERS (1) ---- (1) PROFILES
  AUTH.USERS (1) ---- (N) STOCK_MOVEMENTS
```

---

# 🧪 Descritivo de Casos de Teste de Software

### **CT01 – Cadastrar Produto**

**Objetivo:** Validar o cadastro de um produto.
**Pré-condição:** Usuário autenticado.
**Resultado esperado:** Produto é criado com estoque inicial 0 e datas automáticas.

---

### **CT02 – Editar Produto**

**Objetivo:** Atualizar dados de um produto existente.
**Resultado esperado:** Campo `updated_at` é atualizado automaticamente.

---

### **CT03 – Excluir Produto**

**Objetivo:** Remover um produto.
**Resultado esperado:** Movimentações associadas são removidas (ON DELETE CASCADE).

---

### **CT04 – Listar Produtos**

**Objetivo:** Exibir todos os produtos.
**Resultado esperado:** Produtos são exibidos conforme regras RLS.

---

### **CT05 – Registrar Movimentação de Entrada**

**Resultado esperado:** Estoque aumenta conforme quantidade registrada.

---

### **CT06 – Registrar Movimentação de Saída**

**Resultado esperado:** Estoque diminui corretamente.

---

### **CT07 – Impedir Movimentação com Quantidade Inválida**

**Resultado esperado:** Quantidade 0 ou negativa deve ser rejeitada pelo banco.

---

### **CT08 – Impedir Movimentação de Outro Usuário**

**Resultado esperado:** RLS impede inserir movimentações com `user_id` diferente do usuário logado.

---

### **CT09 – Criar Perfil Automaticamente**

**Resultado esperado:** Trigger cria perfil automaticamente ao criar usuário.

---

### **CT10 – Editar Apenas o Próprio Perfil**

**Resultado esperado:** RLS impede edição de perfis de outros usuários.

---

### **CT11 – Listar Movimentações**

**Resultado esperado:** Todas as movimentações são exibidas para usuários autenticados.

---

### **CT12 – Verificar Estoque Mínimo**

**Resultado esperado:** Sistema indica produtos abaixo do estoque mínimo.

---

### **CT13 – Validar Datas Automáticas**

**Resultado esperado:** `created_at` e `updated_at` são gerados automaticamente via trigger.

