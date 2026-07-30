# Banco de Dados Relacionais com SQL

## Objetivo do projeto
Este repositório reúne os principais conceitos de bancos de dados relacionais e SQL, incluindo modelagem de dados, comandos fundamentais, consultas avançadas, normalização, relacionamentos entre tabelas e um banco de exemplo completo para estudos e prática.

> **Observação:** Todos os exemplos utilizam a sintaxe do **MariaDB/MySQL**. Alguns comandos podem variar em outros SGBDs, como PostgreSQL, SQL Server e Oracle.

## Índice
- Conceitos Básicos
- Introdução ao SQL
- MER e DER
- Modelagem de Dados
- CRUD
- Alteração de Estrutura
- Chaves
- Normalização
- Consultas Avançadas
- JOINs
- Funções Agregadas
- Índices
- Banco de Dados de Exemplo
- Exemplos de Consultas
- Resumo

---

# Conteúdo

## 1. Conceitos Básicos e Estrutura do Banco de Dados Relacional

- **Banco de Dados Relacional**: sistema de organização de dados em tabelas que se relacionam por meio de chaves. Os dados são estruturados em linhas e colunas e são armazenados de forma consistente e interconectada.
- **Tabela**: coleção de registros relacionados, organizada em linhas e colunas. Cada tabela representa uma entidade ou conceito do domínio do problema.
- **Registro**: linha de uma tabela que contém os dados de uma instância específica da entidade. Também é chamado de tupla.
- **Coluna**: elemento vertical da tabela que define um tipo de informação a ser armazenado. Cada coluna possui um nome e um tipo de dado.
- **Campo**: interseção entre uma coluna e um registro, ou seja, o valor de um atributo em uma linha específica.
- **SGBD**: Sistema de Gerenciamento de Banco de Dados. Software que gerencia a criação, leitura, atualização e exclusão de dados em um banco de dados.
- **Principais bancos relacionais**: MySQL, MariaDB, PostgreSQL, SQL Server e Oracle.

Esses SGBDs oferecem suporte ao SQL e são usados em aplicações empresariais, web, comerciais e de pesquisa.

---

## 2. Introdução ao SQL

- **SQL**: Structured Query Language, linguagem padrão para interagir com bancos de dados relacionais.
- **DDL** (Data Definition Language): comandos que definem e alteram a estrutura do banco, como `CREATE`, `DROP`, `ALTER`.
- **DML** (Data Manipulation Language): comandos que manipulam dados, como `INSERT`, `UPDATE`, `DELETE`.
- **DQL** (Data Query Language): comandos de consulta, principalmente `SELECT`.
- **DCL** (Data Control Language): comandos de controle de acesso, como `GRANT`, `REVOKE`.
- **TCL** (Transaction Control Language): comandos de controle de transações, como `COMMIT`, `ROLLBACK`, `SAVEPOINT`.

### Principais comandos

- `CREATE DATABASE`: cria um banco de dados.
- `USE`: seleciona o banco de dados ativo.
- `CREATE TABLE`: cria uma tabela.
- `DROP DATABASE`: remove um banco de dados.
- `DROP TABLE`: remove uma tabela.

### Exemplos

```sql
CREATE DATABASE biblioteca;
USE biblioteca;

CREATE TABLE usuarios (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    email VARCHAR(120) UNIQUE NOT NULL,
    data_cadastro DATE
);

DROP TABLE usuarios;
DROP DATABASE biblioteca;
```

---

## 3. MER e DER

- **MER**: Modelo Entidade-Relacionamento. Representação conceitual dos dados, mostrando entidades, atributos e relacionamentos.
- **DER**: Diagrama Entidade-Relacionamento. Representação visual do MER, em formato de diagrama.
- **Entidade**: objeto ou conceito significativo do domínio, como `Usuario`, `Livro`, `Pedido`.
- **Atributo**: característica ou propriedade da entidade, como `nome`, `email`, `data_nascimento`.
- **Relacionamento**: associação entre duas entidades, por exemplo, um usuário pode fazer um empréstimo de livro.
- **Cardinalidade**: define a quantidade de instâncias de uma entidade que podem estar relacionadas com outra.
  - `1:1` (um para um): cada registro de A se relaciona com no máximo um registro de B.
  - `1:N` (um para muitos): um registro de A pode se relacionar com muitos registros de B.
  - `N:N` (muitos para muitos): muitos registros de A podem se relacionar com muitos registros de B.

Diagrama ASCII simples:

```text
[Usuario] 1 --- N [Emprestimo] N --- 1 [Livro]
```

Nesse exemplo, um usuário pode ter vários empréstimos, e um livro pode aparecer em vários empréstimos.

---

## 4. Modelagem de Dados

- **Tabelas**: estruturas que armazenam dados de entidades.
- **Colunas**: definem os tipos de dados e o formato das informações.
- **Registros**: cada linha representa uma instância de uma entidade.
- **Tipos de dados mais comuns**:
  - `INT`: inteiro.
  - `VARCHAR(n)`: texto de tamanho variável.
  - `DATE`: data.
  - `DECIMAL(p, s)`: número com precisão e escala.
  - `BOOLEAN`: valor lógico verdadeiro/falso.
  - `TEXT`: texto de tamanho livre.

### Exemplos de tipos de dados

```sql
CREATE TABLE exemplo (
    id INT PRIMARY KEY,
    nome VARCHAR(100),
    data_nascimento DATE,
    preco DECIMAL(10,2),
    ativo BOOLEAN,
    observacoes TEXT
);
```

---

## 5. Operações CRUD

### CREATE (INSERT)

- Insere novos registros em uma tabela.

```sql
INSERT INTO usuarios (nome, email, data_cadastro)
VALUES ('Ana Silva', 'ana.silva@example.com', '2026-07-01');

INSERT INTO livros (titulo, autor_id, editora_id, categoria_id, publicacao_ano, preco)
VALUES ('SQL para Iniciantes', 1, 1, 1, 2024, 79.90);
```

### READ (SELECT)

- Recupera dados de uma ou mais tabelas.

```sql
SELECT * FROM usuarios;
SELECT nome, email FROM usuarios WHERE ativo = TRUE;
```

### UPDATE

- Atualiza dados existentes.

```sql
UPDATE usuarios
SET email = 'ana.nova@example.com'
WHERE id = 1;

UPDATE livros
SET preco = 69.90
WHERE id = 2;
```

### DELETE

- Remove registros.

```sql
DELETE FROM emprestimos
WHERE id = 5;

DELETE FROM usuarios
WHERE id = 10;
```

> Dica: sempre faça backups ou use `SELECT` antes de um `DELETE` para confirmar o conjunto de registros afetados.

---

## 6. Alteração de Estrutura

- `ALTER TABLE`: altera a estrutura de uma tabela.
- `ADD COLUMN`: adiciona uma nova coluna.
- `MODIFY COLUMN`: altera o tipo ou propriedades de uma coluna (MySQL/MariaDB).
- `CHANGE COLUMN`: renomeia e/ou altera a definição de uma coluna (MySQL/MariaDB).
- `DROP COLUMN`: remove uma coluna.
- `RENAME TABLE`: renomeia a tabela.
- `TRUNCATE TABLE`: remove todos os registros da tabela, mantendo a estrutura.
- `DROP TABLE`: remove a tabela e seus dados.

### Exemplos

```sql
ALTER TABLE usuarios
ADD COLUMN telefone VARCHAR(20);

ALTER TABLE usuarios
MODIFY COLUMN email VARCHAR(150) NOT NULL;

ALTER TABLE usuarios
CHANGE COLUMN telefone celular VARCHAR(20);

ALTER TABLE usuarios
DROP COLUMN celular;

RENAME TABLE usuarios TO clientes;

TRUNCATE TABLE emprestimos;

DROP TABLE temp_dados;
```

---

## 7. Chaves

### Primary Key

- Identifica unicamente cada registro.
- Geralmente é uma coluna ou conjunto de colunas.
- Exemplo:

```sql
CREATE TABLE usuarios (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100)
);
```

### Foreign Key

- Referência para uma chave primária em outra tabela.
- Garante integridade referencial.
- Exemplo:

```sql
CREATE TABLE livros (
    id INT PRIMARY KEY AUTO_INCREMENT,
    titulo VARCHAR(150) NOT NULL,
    autor_id INT,
    FOREIGN KEY (autor_id) REFERENCES autores(id)
);
```

### Unique

- Garante que todos os valores em uma coluna sejam únicos.
- Exemplo:

```sql
CREATE TABLE editoras (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(120) UNIQUE
);
```

### Not Null

- Impede valores nulos na coluna.
- Exemplo:

```sql
CREATE TABLE categorias (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(80) NOT NULL
);
```

### Auto Increment

- Gera valores automaticamente para chaves primárias numéricas.
- Exemplo:

```sql
CREATE TABLE emprestimos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    usuario_id INT NOT NULL,
    livro_id INT NOT NULL,
    data_retirada DATE NOT NULL
);
```

> Observação: `AUTO_INCREMENT` é a sintaxe usada por MySQL/MariaDB. Em PostgreSQL usa-se `SERIAL` ou `GENERATED ALWAYS AS IDENTITY`.

---

## 8. Normalização

- **Normalização**: processo de organizar dados para reduzir redundância e dependência.
- Por que utilizar:
  - evita inconsistências;
  - facilita manutenção;
  - melhora a integridade dos dados.
- Problemas da redundância:
  - dados duplicados;
  - atualizações inconsistentes;
  - maior uso de espaço.

### Formas normais

- **1FN** (Primeira Forma Normal): cada campo deve conter apenas um valor atômico e cada registro deve ser único.
- **2FN** (Segunda Forma Normal): deve estar em 1FN e todos os atributos não-chave devem depender da chave inteira.
- **3FN** (Terceira Forma Normal): deve estar em 2FN e todos os atributos não-chave devem depender apenas da chave primária, não de outras colunas não-chave.

### Pequeno exemplo

Má modelagem:

```text
tabela_pedidos
id_pedido | cliente | produto | quantidade | preco_unitario
```

Problemas: redundância de produto e cliente.

Melhor modelagem normalizada:

```text
clientes(id_cliente, nome, email)
produtos(id_produto, nome, preco_unitario)
pedidos(id_pedido, cliente_id, data)
itens_pedido(id_item, pedido_id, produto_id, quantidade)
```

---

## 9. Consultas Avançadas

- `WHERE`: filtra registros.
- `ORDER BY`: ordena resultados.
- `LIMIT`: limita o número de registros retornados.
- `LIKE`: pesquisa por padrão em strings.
- `IN`: compara contra uma lista de valores.
- `BETWEEN`: filtra intervalos.
- `DISTINCT`: remove duplicatas.
- Aliases: apelidos para colunas ou tabelas.
- Subconsultas: consultas dentro de outras consultas.
- `EXISTS`: verifica existência de registros.
- `ANY` / `ALL`: compara um valor com qualquer ou todos os valores retornados.

### Exemplos

```sql
SELECT nome, email FROM usuarios
WHERE ativo = TRUE
ORDER BY nome ASC
LIMIT 10;

SELECT DISTINCT categoria_id FROM livros;

SELECT titulo FROM livros
WHERE titulo LIKE '%SQL%';

SELECT * FROM livros
WHERE categoria_id IN (1, 2, 3);

SELECT * FROM emprestimos
WHERE data_retirada BETWEEN '2026-01-01' AND '2026-06-30';

SELECT u.nome AS cliente, l.titulo AS livro
FROM usuarios u
JOIN emprestimos e ON u.id = e.usuario_id
JOIN livros l ON l.id = e.livro_id;

SELECT nome FROM autores a
WHERE EXISTS (
    SELECT 1 FROM livros l WHERE l.autor_id = a.id
);

SELECT titulo FROM livros
WHERE preco > ANY (
    SELECT preco FROM livros WHERE categoria_id = 2
);

SELECT titulo FROM livros
WHERE preco > ALL (
    SELECT preco FROM livros WHERE categoria_id = 3
);
```

---

## 10. JOINs

### INNER JOIN
- Definição: retorna apenas registros com correspondência em ambas as tabelas.
- ASCII:

```text
[TabelaA] ⋂ [TabelaB]
```

- Exemplo:

```sql
SELECT u.nome, e.data_retirada, l.titulo
FROM usuarios u
INNER JOIN emprestimos e ON u.id = e.usuario_id
INNER JOIN livros l ON e.livro_id = l.id;
```

- Resultado esperado: somente empréstimos com usuário e livro existentes.

### LEFT JOIN
- Definição: retorna todos os registros da tabela à esquerda e os correspondentes da direita.
- ASCII:

```text
[TabelaA] ⟕ [TabelaB]
```

- Exemplo:

```sql
SELECT u.nome, e.data_retirada
FROM usuarios u
LEFT JOIN emprestimos e ON u.id = e.usuario_id;
```

- Resultado esperado: todos os usuários, com dados de empréstimo quando existirem.

### RIGHT JOIN
- Definição: retorna todos os registros da tabela à direita e os correspondentes da esquerda.
- ASCII:

```text
[TabelaA] ⟖ [TabelaB]
```

- Exemplo:

```sql
SELECT l.titulo, e.data_retirada
FROM livros l
RIGHT JOIN emprestimos e ON l.id = e.livro_id;
```

- Resultado esperado: todos os empréstimos, com livros quando existirem.

### FULL JOIN
- Definição: retorna registros de ambas as tabelas, mesmo sem correspondência.
- ASCII:

```text
[ A ] ⟗ [ B ]
```

- Exemplo SQL (em bancos que suportam):

```sql
SELECT u.nome, e.data_retirada
FROM usuarios u
FULL JOIN emprestimos e ON u.id = e.usuario_id;
```

- Resultado esperado: usuários e empréstimos combinados, incluindo registros sem par correspondentes.

> Observação: MySQL/MariaDB não suportam `FULL JOIN` diretamente; usam `UNION` de `LEFT JOIN` e `RIGHT JOIN`.

### CROSS JOIN
- Definição: combina todas as linhas de duas tabelas em produto cartesiano.
- ASCII:

```text
[ A ] x [ B ]
```

- Exemplo:

```sql
SELECT u.nome, c.nome AS categoria
FROM usuarios u
CROSS JOIN categorias c;
```

- Resultado esperado: cada usuário combinado com todas as categorias.

---

## 11. Funções Agregadas

- `COUNT`: conta registros.
- `SUM`: soma valores.
- `AVG`: calcula média.
- `MIN`: valor mínimo.
- `MAX`: valor máximo.
- `GROUP BY`: agrupa linhas por valores de coluna.
- `HAVING`: filtra grupos.

### Exemplos

```sql
SELECT COUNT(*) AS total_usuarios FROM usuarios;

SELECT SUM(preco) AS total_livros FROM livros;

SELECT AVG(preco) AS media_preco FROM livros;

SELECT MIN(preco) AS menor_preco, MAX(preco) AS maior_preco FROM livros;

SELECT categoria_id, COUNT(*) AS quantidade
FROM livros
GROUP BY categoria_id;

SELECT categoria_id, AVG(preco) AS media_preco
FROM livros
GROUP BY categoria_id
HAVING AVG(preco) > 50;
```

---

## 12. Índices

- **O que são índices**: estruturas que aceleram buscas em colunas específicas.
- **Quando utilizar**: em colunas usadas frequentemente em `WHERE`, `JOIN` e ordenações.
- **Vantagens**:
  - melhora performance em consultas;
  - reduz tempo de leitura de dados.
- **Desvantagens**:
  - ocupam espaço em disco;
  - tornam `INSERT`, `UPDATE` e `DELETE` mais lentos;
  - devem ser usados com critério.

### Comandos

```sql
CREATE INDEX idx_usuarios_email ON usuarios(email);
DROP INDEX idx_usuarios_email ON usuarios;
```

---

# Banco de Dados de Exemplo

Este exemplo utiliza o banco `biblioteca` com tabelas relacionadas:
- `usuarios`
- `livros`
- `autores`
- `editoras`
- `categorias`
- `emprestimos`

### Criação do banco e tabelas

```sql
CREATE DATABASE biblioteca;
USE biblioteca;

CREATE TABLE autores (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(120) NOT NULL,
    nacionalidade VARCHAR(80),
    data_nascimento DATE
);

CREATE TABLE editoras (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(120) NOT NULL,
    cidade VARCHAR(80),
    pais VARCHAR(80)
);

CREATE TABLE categorias (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(80) NOT NULL UNIQUE
);

CREATE TABLE usuarios (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(120) NOT NULL,
    email VARCHAR(140) NOT NULL UNIQUE,
    telefone VARCHAR(25),
    data_nascimento DATE,
    data_cadastro DATE NOT NULL,
    ativo BOOLEAN NOT NULL DEFAULT TRUE
);

CREATE TABLE livros (
    id INT PRIMARY KEY AUTO_INCREMENT,
    titulo VARCHAR(180) NOT NULL,
    autor_id INT NOT NULL,
    editora_id INT NOT NULL,
    categoria_id INT NOT NULL,
    publicacao_ano INT,
    preco DECIMAL(8,2) NOT NULL,
    estoque INT NOT NULL DEFAULT 0,
    FOREIGN KEY (autor_id) REFERENCES autores(id),
    FOREIGN KEY (editora_id) REFERENCES editoras(id),
    FOREIGN KEY (categoria_id) REFERENCES categorias(id)
);

CREATE TABLE emprestimos (
    id INT PRIMARY KEY AUTO_INCREMENT,
    usuario_id INT NOT NULL,
    livro_id INT NOT NULL,
    data_retirada DATE NOT NULL,
    data_devolucao DATE,
    devolvido BOOLEAN NOT NULL DEFAULT FALSE,
    FOREIGN KEY (usuario_id) REFERENCES usuarios(id),
    FOREIGN KEY (livro_id) REFERENCES livros(id)
);
```

---

## Inserts de exemplo

### Usuários

```sql
INSERT INTO usuarios (nome, email, telefone, data_nascimento, data_cadastro, ativo) VALUES
('Ana Silva', 'ana.silva@example.com', '11987654321', '1990-03-12', '2026-01-10', TRUE),
('Bruno Costa', 'bruno.costa@example.com', '21987654321', '1985-08-02', '2026-02-14', TRUE),
('Carla Pereira', 'carla.pereira@example.com', '31987654321', '1993-11-20', '2026-03-05', TRUE),
('Diego Fernandes', 'diego.fernandes@example.com', '41987654321', '1988-07-30', '2026-04-01', TRUE),
('Elisa Moura', 'elisa.moura@example.com', '51987654321', '1995-05-19', '2026-05-22', TRUE),
('Felipe Ramos', 'felipe.ramos@example.com', '61987654321', '1992-02-18', '2026-06-15', TRUE),
('Gabriela Lima', 'gabriela.lima@example.com', '71987654321', '1989-10-09', '2026-06-30', TRUE),
('Henrique Souza', 'henrique.souza@example.com', '81987654321', '1987-12-01', '2026-07-12', TRUE),
('Isabela Rocha', 'isabela.rocha@example.com', '91987654321', '1994-01-25', '2026-07-20', TRUE),
('João Martins', 'joao.martins@example.com', '11912345678', '1986-04-14', '2026-07-25', TRUE);
```

### Autores

```sql
INSERT INTO autores (nome, nacionalidade, data_nascimento) VALUES
('Paulo Coelho', 'Brasileira', '1947-08-24'),
('Clarice Lispector', 'Brasileira', '1920-12-10'),
('George Orwell', 'Britânica', '1903-06-25'),
('Jane Austen', 'Britânica', '1775-12-16'),
('Gabriel García Márquez', 'Colombiana', '1927-03-06'),
('J.K. Rowling', 'Britânica', '1965-07-31'),
('Stephen King', 'Americano', '1947-09-21'),
('Haruki Murakami', 'Japonesa', '1949-01-12'),
('Chimamanda Ngozi Adichie', 'Nigeriana', '1977-09-15'),
('Yuval Noah Harari', 'Israelense', '1976-02-24');
```

### Editoras

```sql
INSERT INTO editoras (nome, cidade, pais) VALUES
('Editora Alfa', 'São Paulo', 'Brasil'),
('Atlas', 'Rio de Janeiro', 'Brasil'),
('Companhia das Letras', 'São Paulo', 'Brasil'),
('Penguin Random House', 'Londres', 'Reino Unido'),
('HarperCollins', 'Nova York', 'Estados Unidos'),
('Todavia', 'São Paulo', 'Brasil'),
('Rocco', 'São Paulo', 'Brasil'),
('Intrínseca', 'São Paulo', 'Brasil'),
('Leya', 'Lisboa', 'Portugal'),
('Planeta', 'Barcelona', 'Espanha');
```

### Categorias

```sql
INSERT INTO categorias (nome) VALUES
('Ficção'),
('Romance'),
('História'),
('Fantasia'),
('Autoajuda'),
('Tecnologia'),
('Negócios'),
('Ciência'),
('Suspense'),
('Literatura Brasileira');
```

### Livros

```sql
INSERT INTO livros (titulo, autor_id, editora_id, categoria_id, publicacao_ano, preco, estoque) VALUES
('O Alquimista', 1, 3, 5, 1988, 49.90, 8),
('A Hora da Estrela', 2, 3, 10, 1977, 39.90, 5),
('1984', 3, 4, 1, 1949, 59.90, 6),
('Orgulho e Preconceito', 4, 4, 2, 1813, 54.90, 4),
('Cem Anos de Solidão', 5, 4, 1, 1967, 69.90, 7),
('Harry Potter e a Pedra Filosofal', 6, 4, 4, 1997, 79.90, 12),
('O Iluminado', 7, 5, 9, 1977, 59.90, 3),
('Norwegian Wood', 8, 8, 1, 1987, 44.90, 5),
('Americanah', 9, 9, 1, 2013, 64.90, 2),
('Sapiens', 10, 5, 8, 2011, 89.90, 9),
('Brida', 1, 3, 5, 1990, 42.90, 5),
('Laços de Família', 2, 3, 10, 1960, 34.90, 4),
('Revolução dos Bichos', 3, 4, 1, 1945, 47.90, 6),
('Emma', 4, 4, 2, 1815, 38.90, 3),
('Do Amor e Outros Demônios', 5, 6, 5, 2018, 59.90, 4),
('Harry Potter e a Câmara Secreta', 6, 4, 4, 1998, 82.90, 8),
('IT: A Coisa', 7, 5, 9, 1986, 68.90, 2),
('Kafka à Beira-Mar', 8, 8, 1, 2002, 70.00, 3),
('Sejamos Todos Feministas', 9, 9, 10, 2014, 29.90, 6),
('Homo Deus', 10, 5, 8, 2015, 94.90, 7);
```

### Empréstimos

```sql
INSERT INTO emprestimos (usuario_id, livro_id, data_retirada, data_devolucao, devolvido) VALUES
(1, 1, '2026-07-01', '2026-07-10', TRUE),
(2, 2, '2026-07-03', '2026-07-11', TRUE),
(3, 3, '2026-07-04', '2026-07-14', TRUE),
(4, 4, '2026-07-05', NULL, FALSE),
(5, 5, '2026-07-06', NULL, FALSE),
(6, 6, '2026-07-07', '2026-07-17', TRUE),
(7, 7, '2026-07-08', NULL, FALSE),
(8, 8, '2026-07-09', '2026-07-19', TRUE),
(9, 9, '2026-07-10', NULL, FALSE),
(10, 10, '2026-07-11', NULL, FALSE),
(1, 11, '2026-07-12', NULL, FALSE),
(2, 12, '2026-07-13', '2026-07-20', TRUE),
(3, 13, '2026-07-14', NULL, FALSE),
(4, 14, '2026-07-15', NULL, FALSE),
(5, 15, '2026-07-16', NULL, FALSE),
(6, 16, '2026-07-17', NULL, FALSE),
(7, 17, '2026-07-18', NULL, FALSE),
(8, 18, '2026-07-19', NULL, FALSE),
(9, 19, '2026-07-20', NULL, FALSE),
(10, 20, '2026-07-21', NULL, FALSE);
```
```

---

# Exemplos de Consultas

### SELECT *

- Explicação: retorna todas as colunas de uma tabela.

```sql
SELECT * FROM livros;
```

- Resultado esperado: todas as linhas e colunas da tabela `livros`.

### SELECT com WHERE

- Explicação: filtra os resultados de acordo com condições.

```sql
SELECT nome, email FROM usuarios
WHERE ativo = TRUE;
```

- Resultado esperado: usuários ativos.

### ORDER BY

- Explicação: ordena o resultado.

```sql
SELECT titulo, preco FROM livros
ORDER BY preco DESC;
```

- Resultado esperado: livros do mais caro ao mais barato.

### LIMIT

- Explicação: limita o número de registros retornados.

```sql
SELECT * FROM usuarios
ORDER BY data_cadastro DESC
LIMIT 5;
```

- Resultado esperado: 5 usuários mais recentes.

### DISTINCT

- Explicação: remove duplicatas.

```sql
SELECT DISTINCT categoria_id FROM livros;
```

- Resultado esperado: conjunto de categorias usadas pelos livros.

### LIKE

- Explicação: busca por padrão em texto.

```sql
SELECT titulo FROM livros
WHERE titulo LIKE '%Harry%';
```

- Resultado esperado: títulos que contêm 'Harry'.

### BETWEEN

- Explicação: filtra um intervalo.

```sql
SELECT nome, data_retirada FROM emprestimos
WHERE data_retirada BETWEEN '2026-07-01' AND '2026-07-10';
```

- Resultado esperado: empréstimos iniciados na primeira semana de julho.

### IN

- Explicação: compara valores com uma lista.

```sql
SELECT titulo, preco FROM livros
WHERE categoria_id IN (1, 4, 9);
```

- Resultado esperado: livros das categorias selecionadas.

### UPDATE

- Explicação: altera dados existentes.

```sql
UPDATE livros
SET preco = preco * 0.95
WHERE categoria_id = 4;
```

- Resultado esperado: desconto aplicado aos livros de fantasia.

### DELETE

- Explicação: remove registros.

```sql
DELETE FROM emprestimos
WHERE devolvido = TRUE AND data_devolucao < '2026-07-01';
```

- Resultado esperado: remoção de empréstimos já devolvidos antes de julho.

### INNER JOIN

- Explicação: junta registros correspondentes nas duas tabelas.

```sql
SELECT u.nome, l.titulo, e.data_retirada
FROM emprestimos e
INNER JOIN usuarios u ON e.usuario_id = u.id
INNER JOIN livros l ON e.livro_id = l.id;
```

- Resultado esperado: lista de empréstimos com nome de usuário e título do livro.

### LEFT JOIN

- Explicação: retorna todos da tabela à esquerda com correspondentes da direita.

```sql
SELECT u.nome, e.data_retirada
FROM usuarios u
LEFT JOIN emprestimos e ON u.id = e.usuario_id;
```

- Resultado esperado: todos os usuários e seus empréstimos, incluindo usuários sem empréstimos.

### RIGHT JOIN

- Explicação: retorna todos da tabela à direita com correspondentes da esquerda.

```sql
SELECT l.titulo, e.data_retirada
FROM livros l
RIGHT JOIN emprestimos e ON l.id = e.livro_id;
```

- Resultado esperado: todos os empréstimos e os livros correspondentes.

### FULL JOIN

- Explicação: combina registros de ambas as tabelas, mesmo sem correspondência.

```sql
SELECT u.nome, e.data_retirada
FROM usuarios u
FULL JOIN emprestimos e ON u.id = e.usuario_id;
```

- Resultado esperado: usuários e empréstimos combinados, incluindo registros sem par.

> Observação: use `UNION` de `LEFT JOIN` e `RIGHT JOIN` em MySQL/MariaDB.

### CROSS JOIN

- Explicação: produto cartesiano entre duas tabelas.

```sql
SELECT u.nome AS usuario, c.nome AS categoria
FROM usuarios u
CROSS JOIN categorias c;
```

- Resultado esperado: combinações de todos os usuários com todas as categorias.

### Subconsulta

- Explicação: consulta dentro de outra consulta.

```sql
SELECT titulo FROM livros
WHERE autor_id = (
    SELECT id FROM autores WHERE nome = 'J.K. Rowling'
);
```

- Resultado esperado: livros da autora J.K. Rowling.

### COUNT

- Explicação: conta registros.

```sql
SELECT COUNT(*) AS total_emprestimos FROM emprestimos;
```

- Resultado esperado: quantidade total de empréstimos.

### SUM

- Explicação: soma valores.

```sql
SELECT SUM(preco) AS valor_total FROM livros;
```

- Resultado esperado: soma de todos os preços dos livros.

### AVG

- Explicação: média de valores.

```sql
SELECT AVG(preco) AS preco_medio FROM livros;
```

- Resultado esperado: preço médio dos livros.

### MIN

- Explicação: menor valor.

```sql
SELECT MIN(preco) AS menor_preco FROM livros;
```

- Resultado esperado: menor preço entre os livros.

### MAX

- Explicação: maior valor.

```sql
SELECT MAX(preco) AS maior_preco FROM livros;
```

- Resultado esperado: maior preço entre os livros.

### GROUP BY

- Explicação: agrupa registros por coluna.

```sql
SELECT categoria_id, COUNT(*) AS total_livros
FROM livros
GROUP BY categoria_id;
```

- Resultado esperado: total de livros por categoria.

### HAVING

- Explicação: filtra grupos agregados.

```sql
SELECT categoria_id, AVG(preco) AS preco_medio
FROM livros
GROUP BY categoria_id
HAVING AVG(preco) > 60;
```

- Resultado esperado: categorias com preço médio acima de 60.

### CREATE INDEX

- Explicação: cria índice para melhorar performance.

```sql
CREATE INDEX idx_livros_categoria ON livros(categoria_id);
```

- Resultado esperado: melhora a busca por categoria nos livros.

### EXPLAIN

- Explicação: mostra o plano de execução da consulta.

```sql
EXPLAIN SELECT * FROM livros WHERE categoria_id = 1;
```

- Resultado esperado: detalhes de como o banco planeja executar a consulta.

---

# Resumo

| Comando | Finalidade |
|---|---|
| `SELECT` | Recuperar dados |
| `INSERT` | Inserir novos registros |
| `UPDATE` | Atualizar registros existentes |
| `DELETE` | Remover registros |
| `CREATE TABLE` | Criar tabela |
| `ALTER TABLE` | Alterar estrutura de tabela |
| `DROP TABLE` | Excluir tabela |
| `PRIMARY KEY` | Identificador único de registro |
| `FOREIGN KEY` | Chave de referência entre tabelas |
| `INNER JOIN` | Combina registros correspondentes |
| `LEFT JOIN` | Retorna todos da tabela esquerda e correspondentes da direita |
| `RIGHT JOIN` | Retorna todos da tabela direita e correspondentes da esquerda |
| `FULL JOIN` | Retorna registros de ambas as tabelas, mesmo sem correspondência |
| `GROUP BY` | Agrupa registros por coluna |
| `HAVING` | Filtra grupos agregados |
| `COUNT` | Conta registros |
| `SUM` | Soma valores |
| `AVG` | Calcula média |
| `MIN` | Retorna valor mínimo |
| `MAX` | Retorna valor máximo |
| `INDEX` | Acelera buscas em colunas |
