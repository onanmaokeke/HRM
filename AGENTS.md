# AGENTS.md

## Cursor Cloud specific instructions

### Product overview
DWKit HRM is an ASP.NET Core 2.1 web application with a React frontend (Webpack 3). It uses PostgreSQL as the database. See `README.md` for default user credentials and feature list.

### System dependencies (pre-installed in snapshot)
- **.NET Core SDK 2.1.818** — installed at `/usr/share/dotnet`; requires `DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=1` and `OPENSSL_CONF=/dev/null` on Ubuntu 24.04 (ICU 74 and OpenSSL 3 are incompatible with .NET Core 2.1).
- **PostgreSQL 16** — system package; database `hrm` is pre-initialized with the 4 SQL scripts from `DB/PostgreSQL/`.
- **Node.js 22 / npm** — used only for frontend JS bundling (pre-built bundles exist in repo).
- **libssl1.1** — manually installed from Ubuntu 20.04 archive; required by .NET Core 2.1 SSL stack.

### Required environment variables
These are set in `~/.bashrc` and must be present for `dotnet` commands to work:
```
export DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=1
export DOTNET_SKIP_FIRST_TIME_EXPERIENCE=1
export DOTNET_CLI_TELEMETRY_OPTOUT=1
export OPENSSL_CONF=/dev/null
```

### Case-sensitivity gotcha
The solution file `hrm.sln` references the starter project directory as `Optimajet.DWKit.StarterApplication` (lowercase 'j'), but the actual directory is `OptimaJet.DWKit.StarterApplication` (uppercase 'J'). A symlink `Optimajet.DWKit.StarterApplication -> OptimaJet.DWKit.StarterApplication` is committed to fix this on Linux.

### Building
```bash
dotnet build hrm.sln
```
Expect 3 warnings (CS1998 async methods without await) — these are pre-existing.

### Running the application
```bash
# Start PostgreSQL if not running
sudo pg_ctlcluster 16 main start

# Run the app (listens on http://localhost:48801 per launchSettings.json)
cd OptimaJet.DWKit.StarterApplication
dotnet run
```

### Frontend
Pre-built JS bundles (`app.js`, `login.js`, `admin.js`) are committed in `wwwroot/js/`. Only run `npm install` + webpack if modifying frontend code. The `package.json` is at `OptimaJet.DWKit.StarterApplication/package.json`.

### Database
PostgreSQL database `hrm` on localhost:5432, user `postgres`, password `1`. Connection string is in `OptimaJet.DWKit.StarterApplication/appsettings.json`.

### Known limitation
The DWKit demo license expired 2019-12-31. The app runs in demo mode with a form-count warning. This is a license issue, not a code bug.

### Testing
No automated test suite exists in this repository. Verification is manual: build the solution, start the app, log in (admin/1), and navigate the UI.
