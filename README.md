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


•	created_at
•	updated_at
