# Help Desk Ticket Management System

**Enterprise-style full-stack IT support and ticket management platform**

Application No.: IN26009670

A full-stack Help Desk Ticket Management System built with **ASP.NET Core 8, ASP.NET Core MVC, Entity Framework Core, SQL Server, and RESTful APIs**. The application provides a centralized platform for creating, tracking, filtering, and resolving support tickets. It follows a layered architecture with the Repository Pattern, separates the presentation and API layers, and includes automated unit testing with xUnit and Moq.

---

## Table of Contents

- [Overview](#overview)
- [Objectives](#objectives)
- [Key Features](#key-features)
- [System Architecture](#system-architecture)
- [Application Workflow](#application-workflow)
- [Technology Stack](#technology-stack)
- [Project Structure](#project-structure)
- [Core Modules](#core-modules)
- [REST API Reference](#rest-api-reference)
- [Database Architecture](#database-architecture)
- [Design Patterns](#design-patterns)
- [Validation and Error Handling](#validation-and-error-handling)
- [Testing Strategy](#testing-strategy)
- [Getting Started](#getting-started)
- [Configuration](#configuration)
- [Swagger / OpenAPI](#swagger--openapi)
- [Future Enhancements](#future-enhancements)
- [Learning Outcomes](#learning-outcomes)
- [Project Highlights](#project-highlights)
- [Author](#author)
- [License](#license)

---

## Overview

The Help Desk Ticket Management System is a full-stack web application that streamlines IT support operations through centralized ticket management. It enables users to create support tickets, monitor their status, update ticket information, view ticket details, filter tickets, and manage the complete ticket lifecycle.

The solution is composed of two primary applications:

| Project | Type | Responsibility |
|---|---|---|
| `HelpDesk.Api` | ASP.NET Core 8 Web API | RESTful endpoints, request processing, repository interaction, database communication |
| `HelpDesk.Mvc` | ASP.NET Core MVC | User interface, Razor views, ticket forms, dashboard, filtering, API communication via `HttpClient` |

---

## Objectives

- Centralize IT support ticket management
- Provide structured ticket lifecycle management
- Enable efficient ticket tracking and filtering
- Expose reusable, well-documented RESTful APIs
- Maintain clear separation of application responsibilities
- Implement maintainable data access using the Repository Pattern
- Provide persistent storage with SQL Server
- Establish an automated unit testing strategy
- Deliver a responsive, user-friendly web interface

---

## Key Features

### Dashboard
A centralized overview of the current ticket workload, displaying:
- Total tickets
- Open tickets
- In-progress tickets
- Closed tickets

### Ticket Management
Full CRUD-based ticket management:
- **Create** — submit a new support ticket
- **View** — inspect complete details of an individual ticket
- **Update** — modify ticket information as requirements or status change
- **Delete** — remove tickets that are no longer needed
- **List** — browse the complete ticket collection

### Ticket Filtering
Filter tickets by status to help support teams focus on what needs attention:
- All Tickets
- Open
- In Progress
- Closed

### RESTful API
A backend API exposing all core ticket operations, consumed by the MVC front end via `HttpClient`.

---

## System Architecture

The application follows a layered architecture with clear separation of concerns:

```
User
  │
  ▼
ASP.NET Core MVC          (Razor Views, Controllers, ViewModels, Bootstrap UI)
  │  HTTP / HttpClient
  ▼
ASP.NET Core Web API      (API Controllers, Request Processing, HTTP Responses)
  │
  ▼
Repository Layer          (Repository Interfaces & Implementations)
  │
  ▼
Entity Framework Core     (ORM, LINQ, Change Tracking, Migrations)
  │
  ▼
SQL Server (LocalDB)      (Ticket Data)
```

### Architectural Responsibilities

| Layer | Responsibility |
|---|---|
| MVC Presentation Layer | User interface and interaction |
| MVC Controller Layer | Handles browser requests, coordinates UI operations |
| Web API Layer | Exposes RESTful endpoints |
| Repository Layer | Abstracts database access |
| Entity Framework Core | ORM and database communication |
| SQL Server | Persistent data storage |
| Testing Layer | Automated application validation |

This structure promotes separation of concerns, maintainability, testability, reduced coupling, and scalability.

---

## Application Workflow

**Request-response flow:**

```
User Action → MVC Controller → HttpClient → REST API Endpoint
→ API Controller → Repository → Entity Framework Core → SQL Server
→ Database Response → API Response → MVC Controller → Razor View → User Interface
```

**Example: creating a ticket**

```
User submits ticket form
  → MVC Controller (HTTP POST)
    → ASP.NET Core Web API
      → Ticket Repository
        → Entity Framework Core
          → SQL Server (persist ticket)
        ← API Response
      ← MVC Controller
    ← Updated ticket list rendered in the UI
```

---

## Technology Stack

**Backend**

| Technology | Purpose |
|---|---|
| C# | Primary programming language |
| ASP.NET Core 8 | Backend framework |
| ASP.NET Core Web API | RESTful services |
| Entity Framework Core 8 | Object-relational mapping |
| SQL Server LocalDB | Relational database |
| Repository Pattern | Data access abstraction |

**Frontend**

| Technology | Purpose |
|---|---|
| ASP.NET Core MVC | Presentation framework |
| Razor Views | Server-side UI rendering |
| Bootstrap 5 | Responsive interface |
| HTML5 / CSS3 | Page structure and styling |
| HttpClient | API communication |

**Testing**

| Technology | Purpose |
|---|---|
| xUnit | Unit testing framework |
| Moq | Dependency mocking |
| Test Explorer | Test execution and reporting |

**Development Tools**

- Visual Studio 2022
- .NET 8 SDK
- Swagger / OpenAPI
- Git & GitHub
- SQL Server LocalDB

---

## Project Structure

```
HelpDeskManagement
├── HelpDesk.Api
│   ├── Controllers/
│   │   └── TicketController.cs
│   ├── Data/
│   │   └── ApplicationDbContext.cs
│   ├── Models/
│   │   └── Ticket.cs
│   ├── Repositories/
│   │   ├── ITicketRepository.cs
│   │   └── TicketRepository.cs
│   ├── Migrations/
│   ├── Program.cs
│   └── appsettings.json
│
├── HelpDesk.Mvc
│   ├── Controllers/
│   │   └── TicketController.cs
│   ├── Models/
│   ├── Services/
│   ├── Views/
│   │   ├── Home/
│   │   └── Ticket/
│   ├── wwwroot/
│   │   ├── css/
│   │   ├── js/
│   │   └── images/
│   └── Program.cs
│
├── HelpDesk.Tests
│   ├── Controllers/
│   └── TicketControllerTests.cs
│
├── HelpDeskManagement.sln
├── README.md
└── .gitignore
```

---

## Core Modules

**Dashboard Module** — aggregates ticket data into Total, Open, In Progress, and Closed counts for a quick operational summary.

**Ticket Management Module** — Create, View, Update, and Delete operations on individual tickets.

**Filtering Module** — narrows the ticket list by status: Open, In Progress, or Closed.

---

## REST API Reference

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/tickets` | Retrieve all tickets |
| `GET` | `/api/tickets/{id}` | Retrieve a ticket by ID |
| `POST` | `/api/tickets` | Create a new ticket |
| `PUT` | `/api/tickets/{id}` | Update an existing ticket |
| `DELETE` | `/api/tickets/{id}` | Delete a ticket |
| `GET` | `/api/tickets/status/{status}` | Retrieve tickets by status |

---

## Database Architecture

The application uses **SQL Server LocalDB** for persistent storage, with **Entity Framework Core** acting as the ORM layer.

```
Application → EF Core (DbContext, DbSet, LINQ, Change Tracking, Migrations) → SQL Server LocalDB → Ticket Data
```

### Entity Model

The primary domain entity is `Ticket`, representing an individual support request:

- Ticket ID
- Title
- Description
- Status
- Additional ticket metadata (as defined in the current `Ticket` model)

Database migrations are applied via:

```bash
Update-Database
```

---

## Design Patterns

### Repository Pattern

Abstracts data-access logic away from the API controllers:

```
API Controller → ITicketRepository → TicketRepository → ApplicationDbContext → SQL Server
```

**Benefits:** separation of concerns, reduced coupling, improved testability, centralized data-access logic, simplified mocking in unit tests.

### MVC Pattern

```
User Request → Controller → (Model | Service) → View → User Interface
```

- **Model** — represents application and view data
- **View** — Razor-based UI rendering
- **Controller** — processes requests, coordinates operations, and returns the appropriate view

---

## Validation and Error Handling

Clear boundaries are maintained between the presentation, API, and data-access layers:

- **API layer** — request processing, resource handling, HTTP responses, repository interaction, database operations
- **MVC layer** — user interaction, form submission, API communication, response rendering, UI-level validation

For production deployment, centralized exception handling and structured logging are recommended additions (see [Future Enhancements](#future-enhancements)).

---

## Testing Strategy

Automated unit testing is implemented with **xUnit** and **Moq**, with Moq used to mock repository dependencies so controllers can be tested independently of the database.

```
Unit Test → Controller → Mock Repository → Test Result
```

Coverage focuses on controller behavior, repository interactions, CRUD operations, API response handling, and edge-case scenarios.

**Run tests via Visual Studio:** Test → Test Explorer → Run All Tests

**Run tests via CLI:**

```bash
dotnet test
```

---

## Getting Started

You'll need Visual Studio 2022, the .NET 8 SDK, and SQL Server LocalDB installed.

Because the solution is split into two runnable projects (`HelpDesk.Api` and `HelpDesk.Mvc`), a few things need to be set up before the app runs correctly.

**Database connection**
Open `HelpDesk.Api/appsettings.json` and confirm the LocalDB connection string points to a database name you're comfortable with — the default works out of the box for most local setups.

**Apply migrations**
From the Package Manager Console (Tools → NuGet Package Manager → Package Manager Console), with `HelpDesk.Api` set as the default project:

```powershell
Update-Database
```

This creates the database and applies the current schema.

**Run both projects together**
The MVC front end talks to the API over HTTP, so both need to be running at once. In Visual Studio, go to Solution → Properties → Multiple Startup Projects and set both `HelpDesk.Api` and `HelpDesk.Mvc` to **Start**.

**Launch**
Press F5. The API starts first (check the console/Swagger URL it binds to), and the MVC app opens in the browser pointing at the dashboard.

If the MVC app can't reach the API, double-check that the API's base URL in `HelpDesk.Mvc` (typically in `appsettings.json` or a services configuration file) matches the port the API is actually running on — this is the most common setup issue with a two-project split like this.

---

## Configuration

Application configuration is primarily maintained in `HelpDesk.Api/appsettings.json`, covering:

- Database connection strings
- Logging configuration
- API configuration
- Environment-specific settings

> **Security note:** Do not commit passwords, API keys, authentication secrets, production connection strings, or other credentials to source control. Use environment variables or a secure secret-management solution in production.

---

## Swagger / OpenAPI

The API integrates Swagger/OpenAPI for interactive documentation and testing — explore endpoints, inspect parameters, and submit requests directly from the browser once the API is running.

---

## Future Enhancements

The current implementation covers core help-desk functionality. Planned or possible enhancements toward a more comprehensive enterprise-grade ITSM platform include:

**Authentication & Authorization**
- ASP.NET Core Identity with Role-Based Access Control (Administrator, Support Agent, Employee)
- Permission-scoped ticket operations and secure session management

**Advanced Ticket Management**
- Priority levels (Low / Medium / High / Critical) and categories (Hardware / Software / Network / Access)
- Agent/department assignment, SLA deadlines, automatic escalation
- Comments, internal notes, and file attachments

**Search & Filtering**
- Global search; filters by priority, category, agent, and date; pagination and sorting

**Notifications**
- Email and in-app notifications for ticket creation, status changes, assignment, and SLA breaches

**Analytics & Reporting**
- Ticket volume trends, resolution/response time metrics, agent performance, SLA compliance
- PDF/Excel/CSV export

**AI-Assisted Support**
- Automatic categorization and priority prediction, intelligent routing, duplicate detection, suggested solutions, AI-generated summaries, natural-language search

**Integrations**
- Microsoft Teams, Slack, email, Microsoft 365, enterprise identity providers, monitoring/alerting systems

**Cloud & DevOps**
- Migration to Azure (App Service, Azure SQL, Key Vault), Docker/Docker Compose, CI/CD with GitHub Actions

**Security Hardening**
- JWT / OAuth 2.0 / OpenID Connect, HTTPS enforcement, rate limiting, input sanitization, audit logging

**Performance & Scalability**
- Async database operations, indexing, response/API caching, Redis, optimized EF Core queries

**Broader ITSM Capabilities**
- Incident, Problem, and Change Management; Service Catalog; Knowledge Base; Asset Management; Audit Trails

---

## Learning Outcomes

This project provided hands-on experience with full-stack .NET development, including:

C#, ASP.NET Core 8, ASP.NET Core MVC, RESTful API design, Entity Framework Core, SQL Server, the Repository Pattern, MVC architecture, CRUD operations, HTTP communication, database migrations, Swagger/OpenAPI, unit testing with xUnit and Moq, Git/GitHub workflows, and layered software architecture with separation of concerns.

---

## Project Highlights

| Category | Implementation |
|---|---|
| Application Type | Full-stack web application |
| Domain | IT Support / Service Desk Management |
| Backend | ASP.NET Core 8 Web API |
| Frontend | ASP.NET Core MVC |
| Language | C# |
| ORM | Entity Framework Core 8 |
| Database | SQL Server LocalDB |
| Architecture | Layered architecture |
| Design Pattern | Repository Pattern |
| API Style | RESTful |
| UI Framework | Razor Views + Bootstrap 5 |
| API Documentation | Swagger / OpenAPI |
| Testing Framework | xUnit |
| Mocking Framework | Moq |
| Development Environment | Visual Studio 2022 |
| Version Control | Git / GitHub |

---

## Author

**Harshali Panchal**
Computer Science & Engineering — Artificial Intelligence & Machine Learning
Application No.: IN26009739

---

## License

Developed for academic and educational purposes as part of the Help Desk Ticket Management System assignment.
