# API de Produtos — Integração PHP + PostgreSQL

Projeto simples de backend em PHP puro para cadastro e listagem de produtos, utilizando PDO para conexão com o banco de dados.

Material didático desenvolvido em aula com a turma de Desenvolvimento de Sistemas do SENAI Americana, executado em servidor real.

## 📋 Sobre o projeto

Este projeto é uma API REST básica que permite:

* Cadastrar um novo produto no banco de dados (via `POST`)
* Listar todos os produtos cadastrados, ordenados por ID (via `GET`)

A comunicação é feita em formato JSON, tanto no envio quanto no recebimento dos dados.

## 🗂️ Estrutura do projeto

```
├── conexao.php     # Configurações de conexão com o banco (NÃO versionado)
├── produtos.php    # Rotas de cadastro (POST) e listagem (GET) de produtos
└── api-cep.py      # Script auxiliar de teste — consumo de API externa (ViaCEP)
```

> ⚠️ O arquivo `conexao.php` contém informações sensíveis (host, usuário, senha, nome do banco) e por isso está incluído no `.gitignore`, não sendo enviado ao repositório.

## ⚙️ Tecnologias utilizadas

* PHP (nativo, sem frameworks)
* PDO (PHP Data Objects) para acesso ao banco de dados
* PostgreSQL

## 🔌 Como funciona

O arquivo `produtos.php` verifica o método HTTP da requisição (`$_SERVER["REQUEST_METHOD"]`) e executa a ação correspondente.

### `POST` — Cadastrar produto

Recebe um JSON no corpo da requisição com os campos `nome` e `preco`, e insere um novo registro na tabela `produtos`.

Exemplo de requisição:

```json
{
  "nome": "Teclado Mecânico",
  "preco": 250.00
}
```

Resposta:

```json
{
  "Mensagem": "Produto cadastrado com sucesso!"
}
```

### `GET` — Listar produtos

Retorna todos os produtos cadastrados, em formato JSON, ordenados por `id`.

Resposta:

```json
[
  {
    "id": 1,
    "nome": "Teclado Mecânico",
    "preco": "250.00"
  }
]
```

> 💡 Observação: o campo `preco` retorna como **string** (`"250.00"`) e não como número. Isso acontece porque o driver PDO do PostgreSQL entrega valores `DECIMAL`/`NUMERIC` em texto, para não perder precisão na conversão para `float`. Se o front-end precisar do valor numérico, faça a conversão explícita no lado que consome a API.

## 🛠️ Configuração do banco de dados

Crie um arquivo `conexao.php` na raiz do projeto com o seguinte conteúdo (ajuste conforme seu ambiente):

```php
<?php

$host   = "localhost";
$dbname = "nome_do_banco";
$user   = "usuario";
$senha  = "senha";

$pdo = new PDO(
    "pgsql:host=$host;port=5432;dbname=$dbname",
    $user,
    $senha
);

// Faz o PDO lançar exceção em caso de erro, em vez de falhar silenciosamente
$pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
```

E crie a tabela `produtos` no seu banco:

```sql
CREATE TABLE produtos (
    id    INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    nome  VARCHAR(100) NOT NULL,
    preco DECIMAL(10,2) NOT NULL
);
```

> 📝 Atenção ao dialeto: `AUTO_INCREMENT` é sintaxe do **MySQL** e não funciona no PostgreSQL. No Postgres o equivalente é `SERIAL` ou, a partir da versão 10, `GENERATED ALWAYS AS IDENTITY` — que é o padrão SQL e impede que alguém insira um `id` manualmente e desalinhe o contador da sequence.

## ▶️ Como rodar o projeto

1. Clone o repositório
2. Crie o arquivo `conexao.php` com suas credenciais (veja a seção acima)
3. Crie a tabela `produtos` no banco
4. Inicie um servidor local:

```bash
php -S localhost:8000
```

5. Faça as requisições para `http://localhost:8000/produtos.php`

### Testando com curl

```bash
# Cadastrar
curl -X POST http://localhost:8000/produtos.php \
  -H "Content-Type: application/json" \
  -d '{"nome":"Teclado Mecânico","preco":250.00}'

# Listar
curl http://localhost:8000/produtos.php
```

## 📌 Próximos passos (ideias de melhoria)

* Adicionar validação dos dados recebidos
* Implementar métodos `PUT` (atualizar) e `DELETE` (remover)
* Adicionar tratamento de erros mais detalhado
* Criar autenticação para proteger as rotas
* Mover as credenciais para variáveis de ambiente (`.env`)

## 📄 Licença

Distribuído sob a licença MIT. Sinta-se livre para usar, modificar e compartilhar, inclusive em sala de aula.
