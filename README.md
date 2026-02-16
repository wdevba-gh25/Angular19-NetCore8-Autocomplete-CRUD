# Angular 19 + ASP.NET Core 8 Autocomplete + CRUD (SQL Server)

Runnable demo showing:
- **Angular 19** UI with a custom **autocomplete** (debounce, keyboard/mouse UX, parent→child eventing)
- **ASP.NET Core 8** Web API backend
- **SQL Server** database
- Optional **Employee CRUD** screen (router-based) calling the same API

> This repo is meant to be **practical and runnable**. It is not trying to be “enterprise perfect”.

---

## Contents
- [What this demo shows](#what-this-demo-shows)
- [Prerequisites](#prerequisites)
- [Quick start](#quick-start)
  - [1) Database](#1-database)
  - [2) API](#2-api)
  - [3) Angular app](#3-angular-app)
- [Smoke test (no Swagger required)](#smoke-test-no-swagger-required)
- [How to use the Autocomplete](#how-to-use-the-autocomplete)
- [Enable the Employee CRUD screen](#enable-the-employee-crud-screen)
- [Troubleshooting](#troubleshooting)
- [Notes](#notes)
- [License](#license)

---

## What this demo shows

### Autocomplete (default mode)
- **Debounce**: intentionally waits ~5 seconds before firing the request (demo purpose)
- Rich input handling: `[(ngModel)]`, `(ngModelChange)`, `(keyup)`, `(blur)`, `(focus)`, `(input)`, `[ngClass]`
- List rendering only when results exist
- **Parent/child communication** via `@Output()` + `EventEmitter` when selecting an item

### Employee CRUD (optional mode)
Classic Create / Read / Update / Delete calling the API.

---

## Prerequisites

- .NET SDK 8.x
- Visual Studio 2022 (Community is fine) or Rider
- VS Code (optional)
- SQL Server 2014+ and SSMS
- Node.js (recommended: the version you used for this repo, e.g. **v22.6.0** on Windows 10)

---

## Quick start

### 1) Database

You need a SQL Server database with the table used by autocomplete.

**Expected DB objects (minimum):**
- Database: `NG19_TestDb` (or your preferred name)
- Table: `dbo.DropDownListItems`
- Columns:
  - `ddlistId` (int, identity, PK)
  - `itemType` (nvarchar/varchar) e.g. `car_Audi`

**Minimal seed data examples**
- `car_Audi`
- `car_BMW`
- `car_Ford`
- `car_Chery`
- `car_Peugeot`

If you renamed your DB in SSMS, make sure your API connection string matches the real name exactly.

---

### 2) API

1) Open the API solution/project (folder: `API/Employee.Api/...`)
2) Update the connection string used by the API:

`API/Employee.Api/appsettings.json` (and also check `appsettings.Development.json` if present)

```json
{
  "ConnectionStrings": {
    "testCon": "Server=YOUR_SQL_SERVER_HERE;Database=NG19_TestDb;Trusted_Connection=True;TrustServerCertificate=True;MultipleActiveResultSets=true"
  }
}
```

**Common SQL Server values for `Server=`**
- `DESKTOP-SH6IB0S\SQLEXPRESS`
- `(localdb)\MSSQLLocalDB`
- `.\SQLEXPRESS`
- `localhost\SQLEXPRESS`

3) Run the API (HTTPS). Example:
- `https://localhost:7271`

> Swagger is optional. If you want it, enable it yourself locally (not required for this demo).

---

### 3) Angular app

From the Angular app folder (where `package.json` lives):

```bash
npm install
npx ng s
```

Open:
- `http://localhost:4200`

---

## Smoke test (no Swagger required)

### Test the API endpoint directly (Windows curl)

```bash
curl -k "https://localhost:7271/api/DropDownListItemsMaster/GetDropDownListItems?param=car_"
```

Expected (example):
```json
[
  { "id": 9,  "itemType": "car_Audi" },
  { "id": 10, "itemType": "car_BMW"  }
]
```

If the API returns data, the DB + API are correctly wired.

---

## How to use the Autocomplete

1) Run **API** + **Angular**
2) Go to `http://localhost:4200`
3) Type one of these and wait ~5 seconds (debounce):
   - `car_` → car brands
   - `count` → countries

You should see a dropdown list similar to the screenshot in this repo.

---

## Enable the Employee CRUD screen

By default, the Autocomplete demo is what you see at `/`.

To switch to the router-based Employee CRUD screen:

1) Stop Angular (`CTRL + C` twice)
2) Edit: `src\app\app.component.html`
   - Comment out existing lines
   - Add:

```html
<router-outlet />
```

3) Edit: `src\app\app.component.ts`
   - Comment out:

```ts
imports: [ListItemDataComponent],
```

   - Add / uncomment:

```ts
imports: [RouterOutlet],
```

4) Start Angular again:

```bash
npx ng s
```

5) Open:

- `http://localhost:4200/employee-data`

**Important validation note**
When Adding/Editing Employee objects:
- The field **Contact No** must be **exactly 10 characters** (no less, no more),
  otherwise Save/Update will silently fail. (Improvement pending: clearer validation messages.)

---

## Troubleshooting

### API returns 500: “server not found / error 53 / Named Pipes Provider”
Cause: wrong SQL Server instance name in the connection string.

Fix:
- Verify the exact Server/Instance you use in SSMS and copy it into `Server=...`.
- Common examples: `DESKTOP-SH6IB0S\SQLEXPRESS` or `(localdb)\MSSQLLocalDB`.

### API returns 500: “Invalid column name 'id'”
Cause: DB column is `ddlistId` but the entity property is `id`.

Fix (recommended): map the property to the real column:

```csharp
[Column("ddlistId")]
public int id { get; set; }
```

(or rename the property back to `ddlistId`).

### Autocomplete UI works but shows no results
- Confirm API returns data via the curl command in **Smoke test**
- Check the Angular API base URL / proxy configuration
- Check browser DevTools Network tab for 404/500/CORS errors

### CORS errors in the browser
Your API must allow `http://localhost:4200` (and/or `https://localhost:4200`).
If you use a named policy, make sure you call it:

```csharp
app.UseCors("allowCors");
```

---

## Notes

- This project is a portfolio/demo artifact.
- If you don’t want to use Swagger locally, you don’t need it. The demo works with direct endpoint calls.
- If you have dependency issues after long time without running the repo, align Node.js version with what this repo expects.

---

## License

MIT (see `LICENSE`).
