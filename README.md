<div align="center">

# 🎯 Upskilling Platform API

**Versioned ASP.NET Core API and SPA dashboard for professional upskilling/reskilling learning paths — the future of work, 2030+.**

![.NET](https://img.shields.io/badge/.NET_8-512BD4?style=for-the-badge&logo=dotnet&logoColor=white)
![C#](https://img.shields.io/badge/C%23-239120?style=for-the-badge&logo=csharp&logoColor=white)
![EF Core](https://img.shields.io/badge/EF_Core_8-512BD4?style=for-the-badge&logo=dotnet&logoColor=white)
![Oracle](https://img.shields.io/badge/Oracle_DB-F80000?style=for-the-badge&logo=oracle&logoColor=white)
![Swagger](https://img.shields.io/badge/Swagger-85EA2D?style=for-the-badge&logo=swagger&logoColor=black)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Render](https://img.shields.io/badge/Render-46E3B7?style=for-the-badge&logo=render&logoColor=black)

**[🌐 Live demo](https://gs-profissoes.onrender.com/)** · **[📖 Swagger](https://gs-profissoes.onrender.com/api-docs)**

<img width="85%" alt="Upskilling Platform dashboard" src="https://github.com/user-attachments/assets/c28984d7-7a8e-4cc5-a190-785aaa4adf0f" />

</div>

---

## ✨ Overview

A capacity-building platform where professionals enroll in **learning paths** (e.g. *Generative AI for Professionals*), track progress, complete or cancel enrollments and rate them. The **ASP.NET Core 8** API uses **URL-based versioning** (v1 for users and learning paths, v2 for enrollments and statistics), **Entity Framework Core** on **Oracle**, and ships with a vanilla JS single-page dashboard and a hardened Docker image deployed on Render.

> The free Render instance may take a few seconds to wake up on the first request.

## 🚀 Features

- 🔢 **API versioning** with `Microsoft.AspNetCore.Mvc.Versioning`: `/api/v1` and `/api/v2`, one Swagger document per version
- 👥 **Users** and 📚 **learning paths** CRUD (v1), with levels `INICIANTE`, `INTERMEDIARIO`, `AVANCADO`
- 📝 **Enrollments** (v2): enroll, update progress (0–100%) and rating (1–5), complete, cancel, list by user or path
- 📊 **Statistics** (v2): totals, active/completed/cancelled enrollments, completion rate, average rating, most popular paths
- ✅ **Business rules**: no duplicate enrollment, completed enrollments are read-only, cancelled ones cannot be completed
- 🧯 **Global exception middleware** mapping `ResourceNotFoundException` → 404 and `BusinessException` → 422
- 🖥️ **SPA dashboard** with client-side routing, live search, form validation, Chart.js metrics and toasts
- 🐳 **Secure container**: multi-stage build, non-root user, read-only filesystem, minimal capabilities, resource limits, health check, log rotation
- 🧪 **Postman collection** covering every endpoint

## 🧱 Tech Stack

| Layer | Technologies |
| --- | --- |
| API | ASP.NET Core 8, C#, API Versioning, Swashbuckle (OpenAPI), FluentValidation, DataAnnotations |
| Data | Entity Framework Core 8, Oracle EF Core provider, versioned SQL scripts |
| Frontend | HTML, CSS, vanilla JavaScript SPA, Chart.js |
| DevOps | Docker, Docker Compose, Render |

## 🏛️ Architecture

```text
Controllers (v1 / v2) ──▶ Services ──▶ Repositories ──▶ AppDbContext (EF Core) ──▶ Oracle
        │                    │
      DTOs          Business exceptions ──▶ ExceptionHandlingMiddleware
```

```text
.
├── src/
│   ├── Controllers/   # Usuarios, Trilhas (v1) · Matriculas, Estatisticas (v2)
│   ├── Services/ Repositories/ Models/ DTOs/ Data/
│   ├── Middleware/    # global exception handling
│   └── Program.cs
├── Migrations/        # V1 schema + V2 seed (Oracle)
├── static/            # SPA dashboard
├── Dockerfile · docker-compose.yml · postman_collection.json
```

<details>
<summary><b>📡 Endpoints</b></summary>

| Version | Resource | Endpoints |
| --- | --- | --- |
| v1 | Users | `GET/POST /api/v1/usuarios` · `GET/PUT/DELETE /api/v1/usuarios/{id}` |
| v1 | Learning paths | `GET/POST /api/v1/trilhas` · `GET/PUT/DELETE /api/v1/trilhas/{id}` |
| v2 | Enrollments | `GET /api/v2/matriculas` · `GET/PATCH/DELETE /api/v2/matriculas/{id}` · `GET /api/v2/matriculas/usuario/{usuarioId}` · `GET /api/v2/matriculas/trilha/{trilhaId}` · `POST /api/v2/matriculas/inscrever` · `POST /api/v2/matriculas/{id}/concluir` · `POST /api/v2/matriculas/{id}/cancelar` |
| v2 | Statistics | `GET /api/v2/estatisticas` |

Enroll a user:

```json
POST /api/v2/matriculas/inscrever
{ "usuarioId": 1, "trilhaId": 2 }
```

</details>

## 🗃️ Database

Oracle tables use the `TRILHAS_` prefix: `USUARIOS`, `TRILHAS`, `COMPETENCIAS`, `TRILHA_COMPETENCIA` (N:N) and `MATRICULAS` (with progress, completion/cancellation dates and rating). Identity keys, cascading FKs, CHECK constraints and indexes are defined in [`Migrations/`](Migrations).

<img src="diagrama%20de%20classes.png" alt="Class diagram" width="80%">

## ⚙️ Getting Started

### 🐳 Docker Compose

```bash
cp .env.example .env     # set DB_CONNECTION_STRING=User Id=...;Password=...;Data Source=host:1521/XEPDB1
docker-compose up -d --build
docker-compose logs -f app
```

### 💻 Local (.NET 8 SDK + Oracle)

```bash
sqlplus user/password@database @Migrations/V1__Initial_Schema_Oracle.sql
sqlplus user/password@database @Migrations/V2__Seed_Data_Oracle.sql
# set ConnectionStrings:DefaultConnection in appsettings.json
dotnet restore
dotnet run
```

| URL | |
| --- | --- |
| `http://localhost:5000` | Dashboard |
| `http://localhost:5000/api/v1` · `/api/v2` | API |
| `http://localhost:5000/api-docs` | Swagger UI |

## 🎓 Academic Context

Built for the **Global Solution 2025** at **[FIAP](https://www.fiap.com.br/)** (Software Engineering), theme *The Future of Work — Upskilling/Reskilling Platform*.
