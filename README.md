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
```

Run the API (Visual Studio or CLI):

```bash
dotnet run --project API/Employee.Api/Employee.Api/Employee.Api.csproj
```

Expected default URLs (from `launchSettings.json`):
- HTTPS: `https://localhost:7271`
- HTTP: `http://localhost:5032`

Sanity check:

```bash
curl -k https://localhost:7271/api/employeemaster
```

### 2) Run the Angular app (Angular 19)
From:

`ng19NP_MAT/`

Install and run:

```bash
npm install
ng serve
```

Open:
- `http://localhost:4200/employee-data` (Employee CRUD)
- `http://localhost:4200/autocomplete` (Autocomplete demo)
- `http://localhost:4200/input` (Input demo)

---

## API endpoints used
- `GET/POST` `https://localhost:7271/api/employeemaster`
- `GET/PUT/DELETE` `https://localhost:7271/api/employeemaster/{id}`

---

## Project structure (high level)

- `ng19NP_MAT/` — Angular 19 app
  - `src/app/employee-data` — employee list + modal CRUD UI
  - `src/app/component/autocomplete` — Material autocomplete demo
  - `src/app/input` — Material input demo
  - `src/app/service` — API calls
- `API/Employee.Api/` — .NET 8 Web API
  - `Controllers/EmployeeMasterController.cs` — CRUD controller
  - `Model/EmployeeDbContext.cs` — EF Core DbContext

---

## Troubleshooting

### Angular shows `net::ERR_TIMED_OUT` calling the API
This usually means the API is not reachable or is blocked/hanging on DB connection.

- Confirm API is running:
  - `https://localhost:7271/weatherforecast`
- Confirm endpoint responds:
  - `curl -k https://localhost:7271/api/employeemaster`
- If the API hangs or errors, re-check the SQL Server instance name:
  - Common values: `MACHINE\\SQLEXPRESS` / `localhost\\SQLEXPRESS`

### API starts but returns 500 (DB/table not found)
This project expects a SQL Server DB with the required tables. If you’re running it on a fresh DB, you’ll need to create the schema (EF migrations or manual script).

### CORS errors in the browser console
If you see CORS blocks after the API is responding, ensure the backend CORS policy is applied consistently. (Some setups require calling `UseCors("allowCors")` in `Program.cs`.)

---

## Notes
This repo is intentionally a **practice/demo** app. The goal is to show Angular Material patterns plus clean API wiring (not a complete business-domain MVP). For production-style work, see my **SaaS ERP / AI Integration** projects.
