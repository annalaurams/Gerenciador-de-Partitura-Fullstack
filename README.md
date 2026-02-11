# Gerenciador de Partituras (Clave)

O Gerenciador de Partituras é uma aplicação web full stack criada para organizar, armazenar e gerenciar partituras musicais.

<div align="center">
<img src="https://img.shields.io/badge/React-61DAFB?style=flat&logo=react&logoColor=black"/>
<img src="https://img.shields.io/badge/TypeScript-3178C6?style=flat&logo=typescript&logoColor=white"/>
<img src="https://img.shields.io/badge/Node.js-339933?style=flat&logo=node.js&logoColor=white"/>
<img src="https://img.shields.io/badge/Express-000000?style=flat&logo=express&logoColor=white"/>
<img src="https://img.shields.io/badge/Prisma-2D3748?style=flat&logo=prisma&logoColor=white"/>
<img src="https://img.shields.io/badge/PostgreSQL-336791?style=flat&logo=postgresql&logoColor=white"/>
<img src="https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white"/>
<img src="https://img.shields.io/badge/Axios-5A29E4?style=flat&logo=axios&logoColor=white"/>

</div>
## Sumário

- [Funcionalidades](#funcionalidades)
- [Tecnologias e Ferramentas](#tecnologias-e-ferramentas)
- [Modelos do Banco de Dados](#modelos-do-banco-de-dados)
- [Como Rodar o Projeto](#como-rodar-o-projeto)
  - [Pré-requisitos](#pré-requisitos)
  - [Configurando o Banco de Dados](#configurando-o-banco-de-dados)
  - [Configurando o Backend](#configurando-o-backend)
  - [Configurando o Frontend](#configurando-o-frontend)
- [Variáveis de Ambiente](#variáveis-de-ambiente)
- [Rotas da API](#rotas-da-api)
- [Tela Principal](#tela-principal)
- [Autora](#autora)

---

## Funcionalidades

### Autenticação
- Cadastro de usuário
- Login com email e senha
- Autenticação via JWT
- Proteção de rotas privadas

### Gerenciamento de Partituras
- Criar partitura com informações detalhadas (nome, compositor, instrumento, tom, descrição)
- Editar partituras existentes
- Excluir partituras
- Listar todas as partituras cadastradas
- Visualizar detalhes completos de cada partitura
- Upload de arquivo da partitura (PDF, PNG, JPG)
- Download do arquivo associado à partitura
- Visualização de arquivo anexado

## Tecnologias e Ferramentas

| Categoria | Tecnologias |
|-----------|-------------|
| **Backend** | Node.js, TypeScript, Express, Prisma ORM, JWT (jsonwebtoken), bcrypt, Multer, ts-node-dev, ESLint |
| **Frontend** | React, TypeScript, React Router DOM, Axios, CSS3 |
| **Banco de Dados** | PostgreSQL |

## Modelos do Banco de Dados

### User (Usuário)

| Campo | Tipo | Descrição |
|-------|------|-----------|
| `id` | UUID | Identificador único do usuário |
| `name` | String | Nome completo do usuário |
| `email` | String | Email do usuário (único) |
| `password` | String | Senha criptografada com bcrypt |
| `createdAt` | DateTime | Data de criação do registro |
| `updatedAt` | DateTime | Data da última atualização |

### Score (Partitura)

| Campo | Tipo | Descrição |
|-------|------|-----------|
| `id` | UUID | Identificador único da partitura |
| `name` | String | Nome da partitura |
| `composer` | String | Nome do compositor |
| `instrument` | String | Instrumento principal |
| `tone` | String | Tom da música |
| `description` | String | Descrição ou observações |
| `filePath` | String | Nome do arquivo no servidor |
| `fileName` | String | Nome original do arquivo |
| `createdAt` | DateTime | Data de criação |
| `updatedAt` | DateTime | Data da última atualização |

## Como Rodar o Projeto

### Pré-requisitos

Certifique-se de ter instalado:
- Node.js (versão 18 ou superior)
- PostgreSQL (versão 12 ou superior)
- npm ou yarn

### Configurando o Banco de Dados

1. Abra o PostgreSQL e crie o banco de dados:
```sql
CREATE DATABASE gerenciador_partituras;
```

2. Anote as credenciais de acesso (usuário, senha, host, porta).

### Configurando o Backend

1. Navegue até a pasta do backend:
```bash

### Rodando com Docker

O projeto está totalmente preparado para rodar com Docker e Docker Compose, facilitando o setup e garantindo que todas as dependências estejam corretas.

#### Pré-requisitos

- Docker
- Docker Compose

#### Passos

1. Clone o repositório e navegue até a raiz do projeto.
2. Execute:
  ```bash
  sudo docker compose up --build
  ```
  Isso irá:
  - Subir o banco de dados PostgreSQL (porta 5433 no host)
  - Subir o backend (porta 3333)
  - Subir o frontend (porta 80)

3. Acesse:
  - Frontend: [http://localhost](http://localhost)
  - Backend: [http://localhost:3333](http://localhost:3333)

#### Sobre as portas
- **Frontend:** Porta 80 (Nginx serve o build do React)
- **Backend:** Porta 3333 (Express/Node)
- **Banco:** Porta 5433 (host) mapeada para 5432 (container)

#### Migrations
As migrations do Prisma são aplicadas automaticamente no banco do container. Caso precise rodar manualmente:
```bash
sudo docker compose exec backend npx prisma migrate deploy
```
Isso garante que as tabelas sejam criadas no banco correto.

#### Arquivos Docker
- **docker-compose.yml:** Orquestra todos os serviços (db, backend, frontend), define volumes, portas e dependências.
- **back/Dockerfile:** Define o ambiente do backend, instala dependências, roda migrations e inicia o servidor.
- **front/my-app/Dockerfile:** Builda o React e serve via Nginx.
- **front/my-app/nginx.conf:** Configuração do Nginx para SPA, redireciona todas as rotas para index.html.

#### SPA e Nginx
O arquivo `nginx.conf` garante que todas as rotas do React sejam tratadas pelo próprio app, evitando erro 404 em rotas como `/register`.

---
| Método | Rota | Descrição |
|--------|------|-----------|
| POST | `/auth/register` | Cadastro de novo usuário |
| POST | `/auth/login` | Login e geração de token JWT |

**Exemplo de resposta do login:**
```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "user": {
    "id": "uuid",
    "name": "Nome do Usuário",
    "email": "email@exemplo.com"
  }
}
```

**Autenticação:** Para acessar as rotas protegidas, inclua o token no header:
```
Authorization: Bearer <seu_token_jwt>
```

### Partituras

Todas as rotas abaixo requerem autenticação JWT.

| Método | Rota | Descrição |
|--------|------|-----------|
| POST | `/scores` | Criar uma nova partitura (com upload de arquivo) |
| GET | `/scores` | Listar todas as partituras do sistema |
| GET | `/scores/:id` | Buscar partitura específica por ID |
| PUT | `/scores/:id` | Atualizar informações da partitura |
| DELETE | `/scores/:id` | Excluir partitura e arquivo associado |

### Arquivos

| Método | Rota | Descrição |
|--------|------|-----------|
| GET | `/files/:filename` | Acessar/baixar arquivo da partitura |

**Formatos de arquivo aceitos:** PDF, PNG, JPG (até 10MB)



## Tela Principal


<p align="center">
  <img src="img/main.png" alt="Tela de Login" width="500" />
</p>


## Autora

Entre em contato: [nalauramoura@gmail.com](mailto:nalauramoura@gmail.com)
