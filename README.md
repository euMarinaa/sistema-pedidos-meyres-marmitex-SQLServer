# 🍽️ Sistema de Pedidos - Meyre's Marmitex (SQL)

Este projeto simula um sistema simples de pedidos para um negócio de marmitas, feito em **SQL Server**. Ele demonstra a criação de tabelas relacionais, inserção de dados e estrutura de controle para pedidos, itens, clientes e pagamentos.

---

## 📦 Estrutura do Banco de Dados

### 🧑‍🤝‍🧑 Clientes
Armazena os dados dos clientes:
- `id`: chave primária, gerada automaticamente
- `nome`: nome do cliente
- `telefone`: telefone de contato

SQL SERVER
CREATE TABLE Clientes (
    id INT IDENTITY(1,1) PRIMARY KEY,
    nome NVARCHAR(100) NOT NULL,
    telefone NVARCHAR(20) NULL
);

INSERT INTO Clientes (nome, telefone) VALUES 
('Ana Souza', '11987654321'),
('Carlos Lima', '11981234567'),
('Julia Silva', '11965845963'),
('João Pereira', '11954879632'),
('Amanda Ramos', '11936254879'),
('Gabriel Barbosa','11923648756'),
('Gabriela Bertodh', '11998547612'),
('Felipe Oliveira', '11968745894'),
('Joice Fagudes','11998745656'),
('Fabiele Rocha','11978456983');
              ----

### 🍛 Pratos
Lista os pratos disponíveis no cardápio:
- `id`: chave primária
- `nome`: nome do prato
- `preco`: valor unitário (com validação de preço positivo)

SQL SERVER
CREATE TABLE Pratos (
    id INT IDENTITY(1,1) PRIMARY KEY,
    nome NVARCHAR(100) NOT NULL,
    preco DECIMAL(10,2) NOT NULL CHECK (preco > 0)
);

INSERT INTO Pratos (nome, preco) VALUES 
('Macarronada com Almodegas', 19.00),
('Strogonofe de Carne', 20.00),
('Feijoada', 16.50),
('Escondidinho de Frango', 17.00),
('Linguiça assada com Batata', 15.00);
              ----

### 📋 Pedidos
Registra pedidos feitos pelos clientes:
- `id`: chave primária
- `cliente_id`: chave estrangeira para a tabela `Clientes`
- `data_pedido`: data e hora do pedido
- `status`: estado do pedido (padrão: `Pendente`)

SQL SERVER
CREATE TABLE Pedidos (
    id INT IDENTITY(1,1) PRIMARY KEY,
    cliente_id INT NOT NULL,
    data_pedido DATETIME DEFAULT GETDATE(),
    status NVARCHAR(20) DEFAULT 'Pendente',
    CONSTRAINT fk_cliente FOREIGN KEY (cliente_id) REFERENCES Clientes(id)
);

INSERT INTO Pedidos (cliente_id, status) 
VALUES (1, 'Pendente'),(2, 'Pendente'),(3, 'Pendente'),(4, 'Pendente'),
(5, 'Pendente'),(6, 'Pendente'),(7, 'Pendente'),(8, 'Pendente'),
(9, 'Pendente'),(10, 'Pendente');
        ----
        
### 🧾 Itens_Pedido
Relaciona os pratos pedidos com suas quantidades:
- `id`: chave primária
- `pedido_id`: chave estrangeira para `Pedidos`
- `prato_id`: chave estrangeira para `Pratos`
- `quantidade`: número de porções (com validação)
- `subtotal`: valor total do item (preço × quantidade)

SQL SERVER
CREATE TABLE Itens_Pedido (
    id INT IDENTITY(1,1) PRIMARY KEY,
    pedido_id INT NOT NULL,
    prato_id INT NOT NULL,
    quantidade INT NOT NULL CHECK (quantidade > 0),
    subtotal DECIMAL(10,2) NOT NULL,
    CONSTRAINT fk_pedido FOREIGN KEY (pedido_id) REFERENCES Pedidos(id),
    CONSTRAINT fk_prato FOREIGN KEY (prato_id) REFERENCES Pratos(id)
);

INSERT INTO Itens_Pedido (pedido_id, prato_id, quantidade, subtotal) VALUES 
(1, 1, 2,  38.00),(2, 3, 1, 16.50), (3,2,3,  60.00), (4,5,1,  15.00),
(6,4, 1,  17.00), (7,3, 2,  33.00), (8, 1, 3,  57.00), (9,5, 2, 30.00),
(10, 3, 4,  66.00), (5,4,2, 34.00);
          ----

### 💵 Pagamentos
Registra os pagamentos efetuados:
- `id`: chave primária
- `pedido_id`: chave estrangeira para `Pedidos`
- `valor_pago`: valor total pago
- `forma_pagamento`: ex: Pix, Débito, Crédito
- `data_pagamento`: data do pagamento

SQL SERVER 
CREATE TABLE Pagamentos (
    id INT IDENTITY(1,1) PRIMARY KEY,
    pedido_id INT NOT NULL,
    valor_pago DECIMAL(10,2) NOT NULL,  -- Inserido manualmente
    forma_pagamento VARCHAR(20) NOT NULL,
    data_pagamento DATETIME DEFAULT GETDATE(),
    FOREIGN KEY (pedido_id) REFERENCES Pedidos(id)
);

INSERT INTO Pagamentos (pedido_id, valor_pago, forma_pagamento) 
VALUES (1, 38.00, 'Pix'), (2, 16.50, 'Pix'), (3, 60.00, 'Debito'), (4, 15.00, 'Debito'),
(5, 34.00, 'Pix'), (6, 17.00,'Credito'), (7, 33.00,'Credito'), 
(8, 57.00,'Debito'), (9,30.00, 'Debito'), (10, 66.00, 'Credito');
              ----

## 📊 Dados de Exemplo

O script insere:
- 10 clientes
- 5 pratos
- 10 pedidos
- Vários itens por pedido
- 10 registros de pagamentos

Esses dados simulam um fluxo real de atendimento em um pequeno negócio de marmitas.

---

## 📌 Objetivo Educacional

Este projeto é ótimo para praticar:
- Criação de tabelas relacionais com restrições (`CHECK`, `FOREIGN KEY`)
- Modelagem de banco de dados para sistemas de venda
- Inserção de dados
- Normalização e integridade referencial

---

## 🚀 Como Usar

1. Copie e cole o código SQL em um ambiente como **SQL Server Management Studio (SSMS)**.
2. Execute os blocos em ordem:
   - Criação das tabelas
   - Inserção de dados
3. Você pode expandir esse projeto com:
   - Views para relatórios
   - Procedures para registrar pedidos dinamicamente
   - Atualização de status (Entregue, Cancelado etc.)

---

## 🛠️ Sugestões de Expansão

- Adicionar status detalhado para o pedido (`Entregue`, `Cancelado`)
- Relatórios de faturamento por mês
- Integração com front-end (ex: em Python ou React)
- Implementar triggers para cálculo automático de `subtotal`

---

## 📬 Contato

Fique à vontade para clonar, estudar e sugerir melhorias! 😄
