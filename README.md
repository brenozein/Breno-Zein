1. Lista de Requisitos Funcionais 

RF01 – Cadastrar produtos
O sistema deve permitir o cadastro de produtos contendo nome, categoria, descrição e demais características técnicas.

RF02 – Editar produtos
O sistema deve permitir a atualização dos dados de um produto já cadastrado.

RF03 – Excluir produtos
O sistema deve permitir excluir um produto do estoque.

RF04 – Listar produtos
O sistema deve permitir que usuários autenticados visualizem todos os produtos cadastrados.

RF05 – Manter controle automático de estoque
O sistema deve recalcular automaticamente o estoque atual de um produto a cada movimentação realizada (entrada ou saída).

RF06 – Registrar movimentações de estoque
O sistema deve permitir registrar movimentações de estoque do tipo entrada ou saída, vinculadas a um produto.

RF07 – Registrar autor da movimentação
Cada movimentação deve conter o ID do usuário responsável.

RF08 – Validar quantidade positiva
O sistema deve aceitar apenas movimentações com quantidade maior que zero.

RF09 – Atualizar estoque automaticamente
Ao registrar uma movimentação:
•	Se entrada, somar ao estoque atual.
•	Se saída, subtrair do estoque atual.

RF10 – Listar movimentações
O sistema deve permitir que usuários autenticados visualizem todas as movimentações de estoque.

RF11 – Criar automaticamente perfil de usuário
Ao criar um usuário na tabela auth.users, o sistema deve gerar automaticamente um perfil em profiles.

RF12 – Editar perfil do usuário
O usuário deve poder alterar apenas seu próprio perfil.

RF13 – Visualizar perfis
Usuários autenticados devem poder visualizar todos os perfis.

RF14 – Verificar estoque mínimo
O sistema deve armazenar um estoque mínimo para cada produto, podendo gerar avisos quando o estoque estiver abaixo do mínimo (implícito pelo campo minimum_stock).

RF15 – Registrar data de criação e atualização
O sistema deve registrar automaticamente os campos created_at e updated_at em produtos, perfis e movimentações.

2. DER – Diagrama Entidade-Relacionamento (Texto + Explicação)

ENTIDADE: Products 
Atributos:
•	id (PK)
•	name
•	description
•	category
•	voltage
•	resolution
•	dimensions
•	storage
•	connectivity
•	minimum_stock
•	current_stock
•	unit_price
•	created_at
•	updated_at

ENTIDADE: Stock_Movements 
Atributos:
•	id (PK)
•	product_id (FK → Products.id)
•	user_id (FK → auth.users.id)
•	movement_type
•	quantity
•	movement_date
•	notes
•	created_at

ENTIDADE: Profiles 
Atributos:
•	id (PK) e (FK → auth.users.id)
•	full_name
•	role
•	create_at
•	update_at

Relacionamentos

1. Products 1—N Stock_Movements
Um produto pode ter várias movimentações de estoque.
Products (1) ---------------- (N) Stock_Movements
Chave estrangeira:
stock_movements.product_id → products.id

2. Users 1—1 Profiles
Cada usuário do sistema possui exatamente um perfil.
auth.users (1) ---------------- (1) Profiles
Chave estrangeira:
profiles.id → auth.users.id

3. Users 1—N Stock_Movements
Um usuário pode registrar várias movimentações.
auth.users (1) ---------------- (N) Stock_Movements
Chave estrangeira:
stock_movements.user_id → auth.users.id

DER em texto estruturado

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

Descritivo de Casos de Teste de Software

A seguir estão os casos de teste funcionais do sistema, alinhados aos requisitos e ao funcionamento do banco de dados.

CT01 – Cadastrar Produto

Objetivo: Validar o cadastro de um produto.
Pré-condições: Usuário autenticado.
Entradas:

name: “Notebook Dell”

category: “notebook”

unit_price: 3500.00

Passos:

Acessar a tela de cadastro de produtos.

Informar os campos obrigatórios.

Confirmar o cadastro.

Resultado esperado:

Produto cadastrado com sucesso.

id gerado automaticamente.

current_stock iniciado em 0.

created_at preenchido automaticamente.

CT02 – Editar Produto

Objetivo: Validar a edição de um produto existente.
Pré-condições: Produto existente no banco.

Passos:

Acessar a tela de edição.

Modificar os campos desejados.

Salvar alterações.

Resultado esperado:

Dados atualizados corretamente.

Trigger atualiza o campo updated_at.

CT03 – Excluir Produto

Objetivo: Validar a exclusão de um produto.
Pré-condições: Produto existente.

Passos:

Selecionar o produto.

Acionar o comando de exclusão.

Resultado esperado:

Produto excluído.

Movimentações relacionadas também excluídas (ON DELETE CASCADE).

CT04 – Listar Produtos

Objetivo: Validar a listagem de produtos.
Pré-condições: Usuário autenticado.

Passos:

Acessar a lista de produtos.

Resultado esperado:

Todos os produtos disponíveis são exibidos.

Regras de segurança RLS aplicadas.

CT05 – Registrar Movimentação de Entrada

Objetivo: Registrar entrada de estoque.
Pré-condições: Produto existente.
Entradas:

movement_type: entrada

quantity: 10

Passos:

Acessar a tela de movimentações.

Selecionar produto e inserir dados.

Confirmar.

Resultado esperado:

Movimentação registrada.

Estoque atualizado automaticamente (somado).

CT06 – Registrar Movimentação de Saída

Objetivo: Registrar saída de estoque.
Pré-condições: Produto com estoque suficiente.
Entradas:

movement_type: saída

quantity: 5

Resultado esperado:

Movimentação registrada.

Estoque atualizado corretamente (subtraído).

CT07 – Impedir Movimentação com Quantidade Inválida

Objetivo: Validar a regra de quantidade positiva.
Entradas inválidas:

quantity = 0

quantity = -3

Resultado esperado:

Operação bloqueada.

CHECK do banco impede o registro.

CT08 – Impedir Movimentação de Outro Usuário

Objetivo: Validar política RLS no INSERT.
Pré-condições: Usuário tenta registrar movimentação com outro user_id.

Resultado esperado:

Inserção bloqueada conforme RLS: auth.uid() = user_id.

CT09 – Criar Perfil Automaticamente ao Criar Usuário

Objetivo: Validar trigger de criação automática de perfil.

Passos:

Criar novo usuário em auth.users.

Resultado esperado:

Registro automático em profiles.

role padrão "user".

CT10 – Editar Apenas o Próprio Perfil

Objetivo: Validar restrição RLS de atualização.

Passos:

Usuário tenta editar o perfil de outro usuário.

Resultado esperado:

Edição bloqueada.

Apenas auth.uid() = id pode editar.

CT11 – Listar Movimentações

Objetivo: Validar exibição de movimentações.

Pré-condições: Movimentações cadastradas.

Resultado esperado:

Usuário autenticado visualiza todas as movimentações.

SELECT permitido pela RLS.

CT12 – Verificar Estoque Mínimo

Objetivo: Validar identificação de produtos abaixo do mínimo.

Pré-condições:

minimum_stock = 10

current_stock = 8

Resultado esperado:

Sistema identifica produto com estoque crítico.

CT13 – Validar datas de criação e atualização

Objetivo: Garantir preenchimento automático de datas.

Passos:

Criar ou atualizar produto, perfil ou movimentação.

Resultado esperado:

created_at preenchido na criação.

updated_at atualizado via trigger.
