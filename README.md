# CleanArchitectureProject (Vehicle Rental System)

A comprehensive .NET 8 vehicle rental management system built with Clean Architecture principles. The platform manages vehicle rentals ("Alquileres"), user registration, vehicle inventory, reviews, and pricing calculations. It implements Domain-Driven Design with CQRS, event sourcing via an Outbox pattern, and containerized infrastructure.

## Technologies

- .NET 8
- ASP.NET Core (Minimal APIs + Controllers)
- PostgreSQL 16 (Entity Framework Core + Npgsql)
- Dapper (for paginated queries)
- MediatR (CQRS)
- FluentValidation
- Serilog + Seq (logging)
- Quartz.NET (background job scheduling)
- JWT Bearer Authentication with permission-based authorization
- API Versioning (Asp.Versioning)
- Docker / Docker Compose
- xUnit, FluentAssertions, Testcontainers (testing)

## Architecture

- **CleanArchitecture.Domain** — Core entities (`Vehiculo`, `Alquiler`, `User`, `Review`), value objects (`DateRange`, `Direccion`, `Moneda`, `Vin`), domain events, repository interfaces, domain errors, and pricing service (`PrecioService`).
- **CleanArchitecture.Application** — CQRS commands/queries, validation via FluentValidation, pipeline behaviors (logging, validation), and pagination abstractions.
- **CleanArchitecture.Infrastructure** — EF Core `ApplicationDbContext`, repository implementations, Dapper SQL connection factory, JWT provider, email service, outbox message processing with Quartz, and API versioning configuration.
- **CleanArchitecture.Api** — Controllers and minimal API endpoints, Swagger with versioned docs, custom exception handling middleware, Serilog configuration, and Dockerfile.

## Key Features

- **Vehicle rental lifecycle**: Reserve, confirm, reject, cancel, and complete rentals with state validation.
- **Dynamic pricing**: `PrecioService` calculates rental cost based on duration, vehicle price, maintenance fees, and optional accessories (Apple CarPlay, Android Auto, air conditioning, maps).
- **Domain events**: Outbox pattern for reliable event publishing — domain events are persisted to an outbox table and processed by a Quartz background job.
- **Permission-based authorization**: Fine-grained access control via custom `PermissionAuthorizationHandler` and `PermissionAuthorizationPolicyProvider`.
- **API versioning**: URL segment versioning (v1) with Swagger documentation per version.
- **PostgreSQL integration**: EF Core with snake_case naming convention for the database schema.
- **Docker support**: PostgreSQL database, Seq log server, and the API itself are all containerized.
- **Comprehensive testing**: Unit tests for domain and application layers, integration tests with Testcontainers, functional API tests, and architecture tests enforcing Clean Architecture rules.

## How to Run

Using Docker Compose:

```bash
docker-compose up -d
```

Or manually with a local PostgreSQL instance:

```bash
dotnet run --project src/CleanArchitecture/CleanArchitecture.Api/CleanArchitecture.Api.csproj
```

The API will be available at `http://localhost:<port>` with Swagger at `/swagger`.

### Infrastructure Dependencies

- PostgreSQL 16 (update connection string in `appsettings.json`)
- Seq (log server — `docker run -d --name seq -e ACCEPT_EULA=Y -p 5341:80 datalust/seq:2023.4`)

## Test Projects

| Project | Type |
|---------|------|
| CleanArchitecture.Domain.UnitTests | Domain entity and value object unit tests |
| CleanArchitecture.Application.UnitTests | Application handler unit tests |
| CleanArchitecture.Application.IntegrationTests | Integration tests with Testcontainers |
| CleanArchitecture.Api.FunctionalTests | API endpoint functional tests |
| CleanArchitecture.ArchitectureTests | Architecture rule enforcement tests |
