# Local Development Guide

## Prerequisites

- PHP 8.3+
- PostgreSQL 16+
- Redis 7+
- Node.js 20+

## Setup

```bash
composer install
cp .env.example .env
php artisan key:generate
php artisan migrate
npm install
npm run build
```

## Running the stack

Open four terminals from `web/`:

| Terminal | Command | Purpose |
|---|---|---|
| 1 | `php artisan serve` | Laravel HTTP server on :8000 |
| 2 | `npm run dev` | Vite HMR on :5173 |
| 3 | `php artisan queue:work` | Queue worker (see note below) |
| 4 | `php artisan reverb:start` | WebSocket server on :8080 |

Or run everything at once (except Reverb) with:

```bash
composer run dev
```

## Queue worker — Windows vs Linux

**On Windows (local dev):** use `php artisan queue:work`

**On macOS / Linux / production:** use `php artisan horizon`

Horizon requires `ext-pcntl` and `ext-posix`, which are Linux/macOS-only PHP extensions.
`composer.json` has `"platform-check": false` so Composer installs Horizon on Windows
without error — but the process supervisor itself won't run. `queue:work` processes jobs
correctly; you just won't have the Horizon dashboard metrics populated during local dev.

The Horizon dashboard at [/horizon](http://127.0.0.1:8000/horizon) loads on all platforms — it
just shows no active workers when running on Windows.

## Running tests

```bash
php artisan test
```

Tests run against the `my_tradie_testing` PostgreSQL database (not SQLite — the schema uses
Postgres-native types). Create it once with:

```sql
CREATE DATABASE my_tradie_testing OWNER my_tradie;
```
