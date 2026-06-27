
## 🗄️ Modelagem do Banco de Dados

Este repositório contém a estrutura de tabelas e cargas iniciais de dados para o sistema de gerenciamento de investimentos da **Ativo Investimentos**.

---

### 📌 Estrutura das Tabelas (DDL)

```sql

-- =============================================

-- TABELAS PRINCIPAIS

-- =============================================


CREATE TABLE perfil (
    id_perfil SERIAL PRIMARY KEY,
    tipo_fase VARCHAR(50) NOT NULL CHECK (
        tipo_fase IN ('Acumulação', 'Multiplicação', 'Preservação de Patrimônio')
    ),
    tipo_perfil VARCHAR(50) NOT NULL
);

CREATE TABLE carteira (
    id_carteira SERIAL PRIMARY KEY
);

CREATE TABLE investimentos (
    id_investimentos SERIAL PRIMARY KEY,
    nome_investimentos VARCHAR(100) NOT NULL,
    tipo_investimentos VARCHAR(50) NOT NULL CHECK (
        tipo_investimentos IN ('Renda Variável', 'Renda Fixa')
    )
);

CREATE TABLE projecao (
    id_projecao SERIAL PRIMARY KEY,
    tipo_prazo VARCHAR(50) NOT NULL CHECK (
        tipo_prazo IN ('Curto/Médio', 'Longo')
    ),
    rentabilidade_esperado NUMERIC(10, 2) NOT NULL,
    id_carteira INT NOT NULL,
    FOREIGN KEY (id_carteira) REFERENCES carteira (id_carteira)
);

CREATE TABLE clientes (
    id_cliente SERIAL PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    cpf VARCHAR(11) NOT NULL,
    email VARCHAR(100) NOT NULL,
    telefone VARCHAR(11) NOT NULL,
    id_carteira INT NOT NULL,
    id_perfil INT NOT NULL,
    FOREIGN KEY (id_carteira) REFERENCES carteira (id_carteira),
    FOREIGN KEY (id_perfil) REFERENCES perfil (id_perfil)
);


-- =============================================

-- TABELAS DE RELACIONAMENTO (N:N)

-- =============================================


CREATE TABLE perfil_investimentos (
    id_investimentos INT NOT NULL,
    id_perfil INT NOT NULL,
    PRIMARY KEY (id_investimentos, id_perfil),
    FOREIGN KEY (id_perfil) REFERENCES perfil (id_perfil),
    FOREIGN KEY (id_investimentos) REFERENCES investimentos (id_investimentos)
);

CREATE TABLE carteira_investimentos (
    id_carteira INT NOT NULL,
    id_investimentos INT NOT NULL,
    PRIMARY KEY (id_carteira, id_investimentos),
    FOREIGN KEY (id_carteira) REFERENCES carteira (id_carteira),
    FOREIGN KEY (id_investimentos) REFERENCES investimentos (id_investimentos)
);

CREATE TABLE projecao_investimentos (
    id_projecao INT NOT NULL,
    id_investimentos INT NOT NULL,
    PRIMARY KEY (id_projecao, id_investimentos),
    FOREIGN KEY (id_projecao) REFERENCES projecao (id_projecao),
    FOREIGN KEY (id_investimentos) REFERENCES investimentos (id_investimentos)
);

## 📥 Carga Inicial de Dados (DML)

SQL

-- =============================================

-- PERFIS

-- =============================================

INSERT INTO perfil (tipo_fase, tipo_perfil)
VALUES
('Acumulação', 'Conservador'),
('Multiplicação', 'Moderado'),
('Preservação de Patrimônio', 'Agressivo');


-- =============================================

-- CARTEIRAS (sem vínculo com perfil)

-- =============================================

INSERT INTO carteira DEFAULT VALUES;
INSERT INTO carteira DEFAULT VALUES;
INSERT INTO carteira DEFAULT VALUES;


-- =============================================

-- CLIENTES

-- =============================================

INSERT INTO clientes (nome, cpf, email, telefone, id_carteira, id_perfil)
VALUES
('João Silva', '12345678901', 'joao.silva@email.com', '11999990001', 1, 1),
('Maria Souza', '23456789012', 'maria.souza@email.com', '11999990002', 2, 2),
('Carlos Lima', '34567890123', 'carlos.lima@email.com', '11999990003', 3, 3);


-- =============================================

-- INVESTIMENTOS

-- =============================================

INSERT INTO investimentos (nome_investimentos, tipo_investimentos)
VALUES
('CDB Banco XP', 'Renda Fixa'),
('Tesouro Direto IPCA+', 'Renda Fixa'),
('Ações Petrobras', 'Renda Variável'),
('Fundo Imobiliário ABCD11', 'Renda Variável');


-- =============================================

-- PROJEÇÕES

-- =============================================

INSERT INTO projecao (tipo_prazo, rentabilidade_esperado, id_carteira)
VALUES
('Curto/Médio', 8.50, 1),
('Curto/Médio', 12.30, 2),
('Longo', 18.75, 3);

-- =============================================

-- RELACIONAMENTOS ENTRE PERFIL E INVESTIMENTOS

-- =============================================

INSERT INTO perfil_investimentos (id_investimentos, id_perfil)
VALUES
(1, 1),
(2, 1),
(3, 3),
(4, 2),
(4, 3);

-- =============================================

-- RELACIONAMENTOS ENTRE CARTEIRA E INVESTIMENTOS

-- =============================================

INSERT INTO carteira_investimentos (id_carteira, id_investimentos)
VALUES
(1, 1),
(2, 2),
(2, 4),
(3, 3),
(3, 4);

-- =============================================

-- RELACIONAMENTOS ENTRE PROJEÇÃO E INVESTIMENTOS

-- =============================================

INSERT INTO projecao_investimentos (id_projecao, id_investimentos)
VALUES
(1, 1),
(2, 2),
(3, 3),
(3, 4);
