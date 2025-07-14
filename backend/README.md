# Nutrihealth API

Este projeto é uma API RESTful desenvolvida com [Flask](https://flask.palletsprojects.com/), utilizando [Docker](https://www.docker.com/) para conteinerização e [PostgreSQL](https://www.postgresql.org/) como banco de dados relacional. Usada pelo frontend do nutrihealth para consulta dos dados no banco de dados.

---

## Estrutura do Projeto

```
.
├── db/
│   ├── modelofisico.sql
│   └── README.md
├── src/
|   |-- models/
|   |   |-- user_model.py
|   │   └── vendas_model.py
|   |-- routes/
|   |   |-- user_route.py
|   │   └── vendas_route.py
│   ├── __init__.py
│   └── config.py
├── .env
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
├── README.md
└── app.py
```

---

## ⚙️ Tecnologias

- Python 3.13+
- Flask
- SQLAlchemy
- PostgreSQL
- Docker / Docker Compose
- dotenv
- psycopg2-binary

---

## Como Executar

### Pré-requisitos

- [Docker](https://www.docker.com/products/docker-desktop)
- [Docker Compose](https://docs.docker.com/compose/)

### 1. Clone o repositório

```bash
git clone https://github.com/ICEI-PUC-Minas-PSG-ADS-TI/psg-ads-2025-1-p5-proj-tiai-t1-nutrihealth.git
cd nutrihealth
```

### 2. Crie o arquivo `.env`

```env
FLASK_DEBUG=0 or 1 to active debug
FLASK_ENV=development or production
DATABASE_URL=postgresql://<user>:<password>@nutrihealth_db:5432/<database>
POSTGRES_DB=
POSTGRES_USER=
POSTGRES_PASSWORD=
```

### 3. Suba os containers

**Se não for a primeira vez rodando o projeto, siga esses passos:**

Se **qualquer um dos arquivos abaixo for modificado**:

- `Dockerfile`
- `docker-compose.yml`
- `requirements.txt`
- `.env`
- `app.py`

Execute:

```bash
docker-compose up --build
```

> Isso garante que todas as dependências e configurações sejam atualizadas corretamente.


Se **apenas arquivos dentro da pasta `src/`** forem modificados, basta rodar:

```bash
docker-compose up
```

> Isso é mais rápido e mantém os containers existentes.

> A API estará disponível em: http://localhost:5000

---

## Rodar Testes

```bash
docker-compose exec nutrihealth pytest
```

---

| Method | Rota                        | Descrição                                         |
|--------|-----------------------------|---------------------------------------------------|
| POST   | /auth/register              | Registers a new user.                             |
| POST   | /auth/login                 | Logs in a user and returns an access token.       |
| GET    | /profile                    | Retrieves the profile of the current authenticated user. |
| GET    | /profile/my_recipes         | Retrieves recipes created by the current user.    |
| GET    | /users                      | Lists all registered users.                       |
| PUT    | /users/<int:id>             | Updates user details by ID.                       |
| DELETE | /users/<int:id>             | Deletes a user by ID.                             |
| POST   | /recipes                    | Creates a new recipe.                             |
| GET    | /recipes                    | Retrieves all recipes.                            |
| GET    | /recipes/<int:recipe_id>    | Retrieves details of a specific recipe by ID.     |
| POST   | /recipes/save/<int:recipe_id> | Saves a recipe to the user's saved recipes.       |
| GET    | /profile/saved_recipes      | Retrieves recipes saved by the current user.      |
| DELETE | /recipes/<int:recipe_id>    | Deletes a recipe by ID.                           |
| GET    | /recipes/user               | Retrieves recipes created by the current authenticated user. |
| POST   | /ingredientes               | Creates a new ingredient.                         |
| GET    | /ingredientes               | Retrieves all ingredients for the current user.   |
| GET    | /ingredientes/<int:ingrediente_id> | Retrieves details of a specific ingredient by ID. |
| PUT    | /ingredientes/<int:ingrediente_id> | Updates an ingredient by ID.                      |
| DELETE | /ingredientes/<int:ingrediente_id> | Deletes an ingredient by ID.                      |
| POST   | /planejamento               | Creates or updates a weekly meal plan entry.      |
| GET    | /planejamento               | Lists the weekly meal plan for the current user.  |
| POST   | /dashboard                  | Saves dashboard data (e.g., monthly report)       |
| GET    | /dashboard                  | Retrieves dashboard data (e.g., monthly report) for a specific month. |

---

## Docker Compose

```yaml
version: "3.8"

services:
  web:
    build: .
    ports:
      - "5000:5000"
    volumes:
      - .:/app
    depends_on:
      - db
    environment:
      - FLASK_ENV=${FLASK_ENV}
      - DATABASE_URL=${DATABASE_URL}

  db:
    image: postgres:15
    restart: always
    environment:
      POSTGRES_DB: ${POSTGRES_DB}
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
    volumes:
      - pgdata:/var/lib/postgresql/data
    ports:
      - "5433:5432"

volumes:
  pgdata:
```

---

## Notas de Desenvolvimento

- Variáveis sensíveis são armazenadas no `.env`
- Código organizado com Blueprints para facilitar expansão

---

## Licença

Este projeto está licenciado sob a [GPL-3.0](../LICENSE).

---

## Autor
Feito por:
- [Guilherme](https://github.com/guilhermehbs).
