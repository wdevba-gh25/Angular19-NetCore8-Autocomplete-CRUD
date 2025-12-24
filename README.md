# Angular 19 + Material UI + .NET 8 — Full-Stack CRUD Demo (Employee Management)

A compact full-stack demo app built to practice **Angular 19 + Angular Material** patterns and wire them to a **.NET 8 Web API** backed by **SQL Server**.

It’s intentionally small (demo/practice scope), but it covers real full-stack mechanics: routing, service calls, modal forms, and a working CRUD API.

## What this demonstrates
- **Angular 19 + Angular Material UI** pages with routing
- **Service-based HTTP integration** to a .NET 8 Web API
- **Employee CRUD** (list + add/edit modal + delete)
- **SQL Server** persistence via EF Core (DbContext)

## Screenshots
> Files already exist under `Screen Captures/` in this repo.

### Employee CRUD
![Employee CRUD](Screen%20Captures/Employee%20CRUD.png)

### Autocomplete (Material)
![Autocomplete](Screen%20Captures/Autocomplete.png)

### Input (Material)
![Input](Screen%20Captures/Input.png)

## Tech stack
- Frontend: **Angular 19**, TypeScript, Angular Material, RxJS
- Backend: **.NET 8 Web API**, Entity Framework Core
- Database: **SQL Server** (Local / Express)

---

## Quickstart (local)

### 1) Run the API (.NET 8)
Open the solution:

`API/Employee.Api/Employee.Api.sln`

Update the connection string in:

`API/Employee.Api/Employee.Api/appsettings.json`

Example for SQL Server Express on your machine:

```json
"ConnectionStrings": {
  "testCon": "Server=HP01\\SQLEXPRESS;Database=NG19_TestDb;MultipleActiveResultSets=true;Trusted_Connection=True;TrustServerCertificate=True"
}
