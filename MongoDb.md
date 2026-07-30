# Atividade: MongoDB e Redis

Este README documenta os principais conceitos e comandos explorados na atividade prática envolvendo o MongoDB (via MongoDB Atlas e Compass) e uma introdução ao Redis.

**Ferramentas utilizadas:**
- [MongoDB Atlas](https://cloud.mongodb.com/) — banco de dados na nuvem
- [MongoDB Compass](https://www.mongodb.com/products/compass) — interface gráfica para gerenciamento do banco
- [JSON Formatter & Validator](https://jsonformatter.curiousconcept.com/) — validação e formatação de documentos JSON

---

## 1. Operações no MongoDB

### 1.1 Conexão com o banco (Atlas)

O banco foi provisionado na nuvem através do MongoDB Atlas, com acesso via usuário e senha configurados no cluster.

> ⚠️ **Atenção:** por segurança, nunca deixe usuário e senha do banco expostos em um README público ou em qualquer arquivo versionado no Git. O ideal é armazenar essas credenciais em variáveis de ambiente (arquivo `.env`, por exemplo) e adicionar esse arquivo ao `.gitignore`.
>
> Exemplo de como referenciar a conexão sem expor a senha:
> ```
> MONGODB_URI=mongodb+srv://<usuario>:<senha>@<cluster>.mongodb.net/
> ```
> Usuário utilizado no projeto: `eliasmendes2908_db_user` (senha armazenada de forma segura, fora do controle de versão).

### 1.2 Criação de coleções

No MongoDB, uma coleção pode ser criada de duas formas:

- **Implícita:** a coleção é criada automaticamente ao inserir o primeiro documento.
  ```js
  db.usuarios.insertOne({})
  ```

- **Explícita:** a coleção é criada manualmente, antes de qualquer inserção.
  ```js
  db.createCollection("destinos")
  ```

### 1.3 Operações CRUD

O MongoDB segue o padrão CRUD (Create, Read, Update, Delete):

| Operação | Método(s)                          |
| -------- | ----------------------------------- |
| Create   | `insertOne`, `insertMany`           |
| Read     | `find`, `findOne`                   |
| Update   | `findOneAndUpdate`                  |
| Delete   | `findOneAndDelete`                  |

### 1.4 Operadores de atualização

Usados junto aos métodos de update para manipular os campos dos documentos:

| Nome                            | Operador    |
| -------------------------------- | ----------- |
| Definir/alterar valor            | `$set`      |
| Remover campo                    | `$unset`    |
| Incrementar valor                | `$inc`      |
| Multiplicar valor                | `$mul`      |
| Adicionar item em array          | `$push`     |
| Remover item de array            | `$pull`     |
| Adicionar item se não existir    | `$addToSet` |

### 1.5 Modelagem de documentos

Exemplo de documento representando um usuário, com dados embutidos (endereços, interesses) e referências (`ObjectId`) para outras coleções:

```json
{
  "_id": 1,
  "nome": "José Elias",
  "idade": 23,
  "data_nascimento": "2002-08-29",
  "enderecos": [
    {
      "logradouro": "Vila Qualquer Coisa",
      "numero": 123,
      "bairro": "Bairro Qualquer Coisa",
      "cidade": "Cidade Qualquer Coisa"
    }
  ],
  "interesses": [
    "kart",
    "culinaria"
  ],
  "reservas": [
    "ObjectId('123')",
    "ObjectId('234')"
  ]
}
```

Exemplo de documento de reserva, relacionando usuário e destino por referência:

```json
{
  "_id": "ObjectId(123)",
  "destino": "ObjectId(456)",
  "data": "2026-12-12",
  "status": "pendente",
  "usuario": "ObjectId(345)"
}
```

Exemplo de documento com dado geoespacial (localização de um destino):

```json
{
  "_id": 1,
  "nome": "Parque Ibirapuera",
  "descricao": "Principal parque de São Paulo",
  "localizacao": {
    "type": "Point",
    "coordinates": [-46.661056, -23.587384]
  }
}
```

---

## 2. Consultas simples

### 2.1 Operadores de consulta

Utilizados dentro do método `find` (ou similares) para filtrar documentos:

| Nome                | Operador |
| -------------------- | -------- |
| Igualdade             | `$eq`    |
| Diferente             | `$ne`    |
| Maior que             | `$gt`    |
| Maior ou igual        | `$gte`   |
| Menor que             | `$lt`    |
| Menor ou igual        | `$lte`   |
| Dentro de uma lista   | `$in`    |
| Fora de uma lista     | `$nin`   |
| E lógico              | `$and`   |
| OU lógico             | `$or`    |
| Negação               | `$not`   |
| Busca por texto       | `$regex` |

### 2.2 Paginação

Para trazer resultados em blocos (páginas), combinam-se os métodos `skip` (pula os primeiros N documentos) e `limit` (limita a quantidade retornada):

```js
db.usuarios.find().skip(10).limit(5)
```

Nesse exemplo, os 10 primeiros documentos são ignorados e os próximos 5 são retornados — útil para implementar paginação em uma listagem.

---

## 3. Breve apresentação do Redis

O Redis (*Remote Dictionary Server*) é um banco de dados **NoSQL** do tipo **chave-valor**, que armazena os dados em memória (in-memory), o que o torna extremamente rápido para leitura e escrita.

Diferente do MongoDB — que é orientado a documentos e voltado para armazenamento persistente e consultas mais complexas — o Redis é usado principalmente para:

- **Cache** de dados de acesso frequente;
- **Sessões** de usuários;
- **Filas** de mensagens e processamento em tempo real;
- **Contadores** e rankings (leaderboards);
- Armazenamento temporário com **expiração automática** de chaves (TTL).

---

## 4. Introdução ao Redis

### 4.1 Estrutura de dados

O Redis armazena informações no formato **chave → valor**, mas os valores podem assumir diferentes estruturas, como:

- **Strings** — valores simples de texto ou números;
- **Lists** — listas ordenadas de elementos;
- **Sets** — conjuntos de elementos únicos, sem ordem;
- **Hashes** — semelhante a um objeto/dicionário, com múltiplos campos e valores;
- **Sorted Sets** — conjuntos ordenados por uma pontuação (score).

### 4.2 Características principais

- Extremamente rápido, pois opera majoritariamente em memória RAM;
- Suporta expiração de chaves (TTL), ideal para caches temporários;
- Simples de usar, com comandos diretos (`SET`, `GET`, `DEL`, `EXPIRE`, entre outros);
- Muito utilizado em conjunto com bancos relacionais ou NoSQL (como o MongoDB) para otimizar performance em aplicações que exigem respostas rápidas.

### 4.3 Principais comandos e exemplos

**Strings**
```
SET nome "José Elias"       # define o valor da chave "nome"
GET nome                    # retorna "José Elias"
SET visitas 0
INCR visitas                # incrementa em 1 (visitas = 1)
INCRBY visitas 5             # incrementa em 5 (visitas = 6)
```

**Expiração de chaves (TTL)**
```
SET sessao "usuario123" EX 60   # define a chave com expiração de 60 segundos
TTL sessao                      # consulta o tempo restante de vida da chave
EXPIRE sessao 120                # redefine o tempo de expiração para 120 segundos
```

**Listas (Lists)**
```
RPUSH interesses "kart"          # adiciona no fim da lista
RPUSH interesses "culinaria"
LRANGE interesses 0 -1           # retorna todos os itens da lista
LPOP interesses                  # remove e retorna o primeiro item
```

**Conjuntos (Sets)**
```
SADD tags "praia" "montanha" "cidade"
SMEMBERS tags                    # lista os itens do conjunto (sem repetição)
SISMEMBER tags "praia"           # verifica se "praia" está no conjunto (1 ou 0)
```

**Hashes (semelhante a um objeto)**
```
HSET usuario:1 nome "José Elias" idade 23
HGET usuario:1 nome              # retorna "José Elias"
HGETALL usuario:1                # retorna todos os campos e valores
```

**Sorted Sets (rankings)**
```
ZADD ranking 100 "jogador1"
ZADD ranking 250 "jogador2"
ZRANGE ranking 0 -1 WITHSCORES   # lista ordenada por pontuação
```

**Chaves em geral**
```
KEYS *                # lista todas as chaves (evitar em produção)
EXISTS nome            # verifica se a chave existe (1 ou 0)
DEL nome               # remove a chave
```

### 4.4 Testando online

É possível testar comandos do Redis diretamente no navegador, sem precisar instalar nada, através do:

🔗 [try.redis.io](https://try.redis.io/)

Basta digitar `TUTORIAL` para iniciar um tutorial guiado, `HELP` para ver a lista de comandos suportados, ou qualquer comando Redis válido para testar diretamente.

---

## 5. Quando usar banco relacional e quando não usar
 
Depois de explorar o MongoDB (NoSQL orientado a documentos) e o Redis (NoSQL chave-valor), vale entender em quais cenários um banco **relacional** (como MySQL, PostgreSQL, SQL Server) ainda é a escolha mais adequada — e quando faz mais sentido optar por um NoSQL.
 
### 5.1 Quando usar banco relacional
 
- Os dados possuem **relacionamentos bem definidos** e estruturados (ex: pedidos, clientes, produtos, pagamentos);
- É necessário garantir **integridade e consistência** forte entre os dados (chaves estrangeiras, restrições, transações **ACID**);
- O esquema (schema) dos dados é **fixo e previsível**, mudando pouco ao longo do tempo;
- A aplicação exige **transações complexas**, como transferências financeiras, onde múltiplas operações precisam ocorrer todas ou nenhuma (ex: sistemas bancários, e-commerce, ERPs);
- É importante realizar **consultas complexas com múltiplos JOINs** entre diferentes tabelas;
- Existe necessidade de **relatórios e auditoria** rigorosos sobre os dados.
### 5.2 Quando não usar banco relacional (optar por NoSQL)
 
- Os dados são **semiestruturados ou variáveis**, sem um formato fixo (ex: perfis de usuário com campos diferentes entre si);
- É necessário **alta escalabilidade horizontal** (distribuir os dados entre vários servidores facilmente), como em aplicações com grande volume de acesso;
- A prioridade é **desempenho e velocidade**, mesmo que isso signifique flexibilizar um pouco a consistência (ex: cache, sessões, contadores — caso do Redis);
- Os dados são naturalmente **hierárquicos ou aninhados**, como documentos com listas e subestruturas (caso do MongoDB, visto na seção 1.5);
- O projeto está em constante evolução e o **esquema muda com frequência**, sem tempo hábil para migrações estruturais custosas;
- A aplicação lida com **grandes volumes de dados não relacionais**, como logs, eventos em tempo real, dados de IoT ou séries temporais.
### 5.3 Resumo comparativo
 
| Critério                        | Banco Relacional (SQL)        | NoSQL (MongoDB / Redis)             |
| -------------------------------- | ------------------------------ | ------------------------------------ |
| Estrutura dos dados               | Fixa (tabelas e colunas)       | Flexível (documentos, chave-valor)   |
| Relacionamentos                   | Fortes (JOINs, chaves estrangeiras) | Geralmente embutidos ou por referência |
| Consistência                      | Forte (ACID)                   | Eventual, na maioria dos casos        |
| Escalabilidade                    | Vertical (mais recursos na mesma máquina) | Horizontal (distribuição entre servidores) |
| Casos de uso típicos               | Sistemas financeiros, ERPs, e-commerce | Catálogos flexíveis, cache, sessões, big data |
 
Na prática, muitos sistemas reais combinam os dois tipos de banco: um relacional para os dados críticos e estruturados, e um NoSQL (como MongoDB e/ou Redis) para dados flexíveis ou que exigem alta performance — foi justamente essa combinação (MongoDB + Redis) que exploramos nesta atividade.
 
---

## Referências

- [MongoDB Atlas](https://cloud.mongodb.com/)
- [MongoDB Compass](https://www.mongodb.com/products/compass)
- [JSON Formatter & Validator](https://jsonformatter.curiousconcept.com/)