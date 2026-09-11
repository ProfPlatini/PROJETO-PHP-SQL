PROJETO PHP + SQL (PostgreSQL)
API RESTful em PHP para gerenciamento de produtos com banco de dados PostgreSQL.
🛠️ Tecnologias Utilizadas
PHP
PostgreSQL (Driver `pgsql` / PDO)
JSON (Comunicação da API)
---
🗄️ Estrutura do Banco de Dados
Crie a base de dados no PostgreSQL com as seguintes especificações:
Banco de Dados: `lojasenai`
Tabela: `produtos`
```sql
CREATE DATABASE lojasenai;

CREATE TABLE produtos (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(255) NOT NULL,
    preco NUMERIC(10, 2) NOT NULL
);
```
---
⚙️ Configuração da Conexão (`conexao.php`)
O arquivo `conexao.php` realiza a conexão via PDO com o servidor PostgreSQL:
```php
$host = "192.168.10.106";
$usuario = "postgres";
$banco = "lojasenai";
$senha = "1234";

$pdo = new PDO(
    "pgsql:host=$host;port=5432;dbname=$banco",
    $usuario,
    $senha
);
```
---
🚀 Endpoints da API (`produtos.php`)
1. Listar Produtos
Método: `GET`
URL: `http://localhost/produtos.php`
Resposta (200 OK):
```json
[
  {
    "id": 1,
    "nome": "Produto Exemplo",
    "preco": "99.90"
  }
]
```
2. Cadastrar Produto
Método: `POST`
URL: `http://localhost/produtos.php`
Header: `Content-Type: application/json`
Corpo da Requisição (Body JSON):
```json
{
  "nome": "Notebook",
  "preco": 3500.00
}
```
Resposta (200 OK):
```json
{
  "Mensagem": "Produto cadastrado com sucesso! 😊"
}
```
---
📂 Arquivos do Projeto
`conexao.php`: Configuração da conexão PDO com PostgreSQL.
`produtos.php`: Processamento das requisições HTTP (GET e POST) e manipulação dos produtos.