# Tradify

Membership platform connecting homeowners and investors with vetted tradespeople.

---

## Repository structure

```
my-tradie/
├── web/                    # Laravel 13 + Inertia.js web app (backend + web frontend)
├── apps/
│   ├── tradie-app/         # React Native tradie mobile app (Phase 2)
│   └── member-app/         # React Native member mobile app (Phase 2)
├── packages/
│   ├── shared/             # Shared API client, types, zod schemas
│   └── ui/                 # Shared React Native UI primitives
├── docs/                   # Engineering documentation (read these before building)
│   ├── 00-README.md
│   ├── 01-product-spec.md
│   ├── ...
│   ├── 11-mobile-apps-spec.md
│   ├── design-mapping.md
│   └── design/             # Stitch-exported screen designs
├── deploy/                 # Deployment scripts
├── CLAUDE.md               # Agent entry point — always read first
├── PROVISIONING.md         # VPS provisioning runbook
├── STAGING.md              # Staging environment setup
├── LEGAL.md                # Legal & compliance checklist
├── GO-TO-MARKET.md         # Launch plan
├── SUPPORT-OPS.md          # Customer support workflow
├── THREAT-MODEL.md         # Security & fraud scenarios
├── SECRETS.md              # Secrets management
├── OBSERVABILITY.md        # Monitoring & runbooks
└── RUNBOOK-CONTINUITY.md   # Business continuity plan
```

---

## Where to start

**If you're Claude Code (the AI agent):**
- Read `CLAUDE.md` first. It points you to the correct doc for any task.

**If you're a human developer:**
- Read `docs/00-README.md` for the reading order
- Read `docs/01-product-spec.md` to understand what we're building
- Read `docs/10-build-phases.md` to see where the project is in its build

**If you're running the business:**
- `LEGAL.md` for the legal checklist
- `GO-TO-MARKET.md` for the launch plan
- `SUPPORT-OPS.md` for customer support operations
- `RUNBOOK-CONTINUITY.md` if something's happened to the founder

---

## Local development prerequisites

### Required
- **PHP 8.3+** with extensions: mbstring, xml, bcmath, pdo_pgsql, redis
- **Composer 2.x**
- **Node.js 20+**
- **pnpm 9+** (`npm install -g pnpm`)
- **PostgreSQL 16+**
- **Redis 7+**
- **Git**

### For mobile work (Phase 2, not needed for Phase 1)
- **Xcode 15+** (macOS only, for iOS builds)
- **Android Studio** (for Android builds)
- **Expo CLI** (installed via `pnpm install`; run via `pnpm tradie:dev`)

### Recommended
- **VS Code** with the Claude Code extension
- **Laravel Sail** if you prefer Dockerised PHP/Postgres/Redis (`web/vendor/bin/sail`)
- **TablePlus** or **DBeaver** for database inspection
- **Postman** or **Insomnia** for API testing

---

## First-time setup

### 1. Clone the repo

```bash
git clone <repo-url> my-tradie
cd my-tradie
```

### 2. Install JS workspace dependencies

```bash
pnpm install
```

This installs dependencies for `apps/*` and `packages/*`. It does NOT touch `web/`.

### 3. Set up the Laravel web app

```bash
cd web
composer install
cp .env.example .env
php artisan key:generate
# Configure .env for your local DB + Redis
php artisan migrate --seed
npm install
npm run dev
php artisan serve
```

(See `docs/02-architecture.md §11` for full local-dev setup including Reverb and Horizon.)

### 4. (Phase 2 only) Set up a mobile app

```bash
cd ..                       # back to my-tradie/
pnpm tradie:dev             # start the tradie app in Expo
```

Scan the QR code with the Expo Go app on your phone, OR press `i` / `a` for iOS / Android simulator.

---

## Phased build

Currently at: **Phase 0 — Foundation setup**

See `docs/10-build-phases.md` for the full phase plan. Each phase has acceptance criteria; do not move on until each is satisfied.

---

## Contributing

Solo founder project at MVP. If you've been added as a contributor:
1. Read the full `docs/` pack before contributing
2. Follow conventions in `docs/09-development-guide.md`
3. PRs only — no direct commits to `main`
4. Run `pnpm typecheck && pnpm lint && pnpm test` before pushing

---

## License

Private / proprietary. Not for redistribution.

---

## Contact

Support: support@tradify.au (once live)
Business: see `RUNBOOK-CONTINUITY.md` for continuity contacts
