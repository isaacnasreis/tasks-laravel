# 🚀 Backend - (Laravel)

Este repositório contém a solução para o desafio técnico de Backend (PHP/Laravel). A aplicação consiste em uma API RESTful para o gerenciamento de tarefas internas.

## 🛠️ Tecnologias e Ferramentas Utilizadas

* **PHP 8.2+**
* **Laravel 12**
* **MySQL**
* **Docker & Laravel Sail** (Ambiente de desenvolvimento isolado)
* **Padrões e Boas Práticas:** REST, SOLID, Clean Code, Form Requests, Eloquent ORM.

## ⚙️ Como executar o projeto (Com Docker)

Este projeto foi construído utilizando o **Laravel Sail**. Para rodar a aplicação, você só precisa ter o [Docker](https://www.docker.com/) instalado em sua máquina.

1. **Clone o repositório:**
   ```bash
   git clone [https://github.com/isaacnasreis/tasks-api.git](https://github.com/isaacnasreis/tasks-api.git)
   cd tasks-api

2. **Configure as variáveis de ambiente:**
   ```bash
   cp .env.example .env

3. **Instale as dependências (via container temporário):**
   ```bash
   docker run --rm \
    -u "$(id -u):$(id -g)" \
    -v "$(pwd):/var/www/html" \
    -w /var/www/html \
    laravelsail/php83-composer:latest \
    composer install --ignore-platform-reqs

4. **Suba os containers em background:**
   ```bash
   ./vendor/bin/sail up -d
   
5. **Prepare a aplicação e o Banco de Dados:**
   ```bash
   ./vendor/bin/sail artisan key:generate
   ./vendor/bin/sail artisan migrate --seed
💡 O comando --seed popula o banco com 15 tarefas para facilitar os testes.

6. **🛑 Para parar a aplicação**
   ```bash
   ./vendor/bin/sail artisan key:generate

## 📚 Documentação da API

Abaixo estão os detalhes de requisição e resposta para os principais endpoints.

📌 Uma collection do Postman (`tasks-api.postman_collection.json`) está disponível na raiz do projeto para testes imediatos.

---

## 1️⃣ Listar Tarefas

### `GET /api/tasks`

### ✅ Retorno (200 OK)

Retorna uma lista paginada (10 itens por página), ordenada pelas mais recentes.

    ```json
    {
      "current_page": 1,
      "data": [
        {
          "id": 1,
          "title": "Configurar servidor",
          "description": "Subir instância na AWS.",
          "completed": false,
          "created_at": "2026-02-27T10:00:00.000000Z",
          "updated_at": "2026-02-27T10:00:00.000000Z"
        }
      ],
      "total": 15
    }

## 2️⃣ Criar Tarefa

### `POST /api/tasks`

### 📌 Headers
    
    ```http
    Accept: application/json
    {
      "title": "Nova Tarefa",
      "description": "Descrição detalhada da tarefa"
    }

### ✅ Retorno (200 OK)
Retorna o objeto criado.

### ❌ Retorno Erro de Validação (422 Unprocessable Entity)
Retorna mensagens traduzidas caso faltem campos obrigatórios.

## 3️⃣ Atualizar Tarefa

### `PUT /api/tasks/{id}`

### 📌 Body
Igual ao de criação, com a adição opcional do campo:

    ```json
    {
      "completed": true
    }

### ✅ Retorno (200 OK)
Retorna a tarefa atualizada.

## 4️⃣ Exibir Tarefa Específica

### `GET /api/tasks/{id}`

### 📌 Body
Igual ao de criação, com a adição opcional do campo:

    ```json
    {
      "completed": true
    }

### ✅ Retorno (200 OK)
Detalhes da tarefa solicitada.

### ❌ Retorno Erro (404 Not Found)

    ```json
    {
      "message": "Registro não encontrado."
    }

## 5️⃣ Excluir Tarefa
### `DELETE /api/tasks/{id}`

✅ Retorno Sucesso (204 No Content)

Sem corpo na resposta.
