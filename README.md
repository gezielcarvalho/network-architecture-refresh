Here's a compact, hands-on "build it up" refresher that starts from a tiny```

> **Naming & networks**
> Use two Do### A. Lucidchart

- Create a new doc: **"Net Refresh – Phase 00"**.
- Add a container **"App Stack"** with network segmentation:
  - Two network boundaries: **front_net** and **back_net**
  - **API** node positioned at the boundary (connected to both networks)
- Add an external **Client** shape → arrow to **API** (HTTP 8080).
- Style: green = stateless, blue = stateful, orange = infra. API = green.
- Include network annotations showing future purpose of each segment.etworks throughout to visualize segmentation:
  > - `front_net` (UI ↔ API)
  > - `back_net` (API ↔ workers/queues/db/iseries/ftp)
  >
  > Add aliases per service (e.g., `api`, `queue`, `db`) so hostnames in code are stable.ep by step—into a microservices system with UI, workers, RabbitMQ, FTP integration, and an iSeries adapter. Each phase has two tracks: (A) diagramming in Lucidchart, (B) implementation with Docker. All service examples are intentionally "hello-world-ish" so you focus on architecture, not app complexity.

---

# Network Architecture Refresh (Hands-On)

## 🎯 Progress Status

| Phase        | Lucidchart  | Implementation | Status      |
| ------------ | ----------- | -------------- | ----------- |
| **Phase 00** | ✅ Complete | ✅ Complete    | ✅ **DONE** |
| Phase 01     | ⏳ Pending  | ⏳ Pending     | ⏳ Next     |
| Phase 02     | ⏳ Pending  | ⏳ Pending     | ⏳ Planned  |

**Phase 00 Completed:**

- ✅ Lucidchart diagram created and exported (`docs/lucid/Net_Refresh_Phase_00.png`)
- ✅ Minimal API with `/health` endpoint (`services/api/server.js`)
- ✅ Docker Compose configuration (`compose/phase-00.yml`)
- ✅ Network setup (`front_net`, `back_net`)
- ✅ Package.json with Express dependency and ES6 modules
- ✅ Step-by-step documentation (`phase-00/step-*.md`)
- ✅ Gitignore configuration
- ✅ Successfully validated: `curl http://localhost:8080/health` → `{"status":"ok"}`

## Repo layout (from Phase 0 onward)```

/network-architecture-refresh
/compose/phase-00.yml # docker-compose files per phase
/phase-00/ # step-by-step guides for phase 00
step-1-lucidchart-phase-00-instructions.md
step-2-docker-api-guide.md
/services
/api # simple REST orchestrator (Node/Express or .NET/Flask – your call)
/ui # minimal UI (static or small SPA)
/worker # background worker (e.g., Python rq / Node BullMQ)
/queue # RabbitMQ (image only)
/cache # Redis (image only)
/db # Postgres (image only)
/ftp # vsftpd container + seed folder
/iseries-adapter # façade that translates to/from iSeries (mocked)
/integrations # tiny mock "partner" services (HTTP)
/.env.example
/.gitignore # excludes node_modules, package-lock.json, etc.
/docs
/lucid/ # exported PNG/PDF per phase
/runbooks/ # short notes per phase

```“build it up” refresher that starts from a tiny API and grows—step by step—into a microservices system with UI, workers, RabbitMQ, FTP integration, and an iSeries adapter. Each phase has two tracks: (A) diagramming in Lucidchart, (B) implementation with Docker. All service examples are intentionally “hello-world-ish” so you focus on architecture, not app complexity.

---

# Network Architecture Refresh (Hands-On)

## 🎯 Progress Status

| Phase | Lucidchart | Implementation | Status |
|-------|------------|----------------|--------|
| **Phase 00** | ✅ Complete | ✅ Complete | ✅ **DONE** |
| Phase 01 | ⏳ Pending | ⏳ Pending | ⏳ Next |
| Phase 02 | ⏳ Pending | ⏳ Pending | ⏳ Planned |

**Phase 00 Completed:**
- ✅ Lucidchart diagram created and exported (`docs/lucid/Net_Refresh_Phase_00.png`)
- ✅ Minimal API with `/health` endpoint (`services/api/server.js`)
- ✅ Docker Compose configuration (`compose/phase-00.yml`)
- ✅ Network setup (`front_net`, `back_net`)
- ✅ Package.json with Express dependency and ES6 modules
- ✅ Step-by-step documentation (`phase-00/step-*.md`)
- ✅ Gitignore configuration
- ✅ Successfully validated: `curl http://localhost:8080/health` → `{"status":"ok"}`

## Repo layout (from Phase 0 onward)

```

/net-architecture-refresh
/compose/phase-00..phase-10 # docker-compose files per phase
/services
/api # simple REST orchestrator (Node/Express or .NET/Flask – your call)
/ui # minimal UI (static or small SPA)
/worker # background worker (e.g., Python rq / Node BullMQ)
/queue # RabbitMQ (image only)
/cache # Redis (image only)
/db # Postgres (image only)
/ftp # vsftpd container + seed folder
/iseries-adapter # façade that translates to/from iSeries (mocked)
/integrations # tiny mock “partner” services (HTTP)
/.env.example
/docs
/lucid/ # exported PNG/PDF per phase
/runbooks/ # short notes per phase

````

> **Naming & networks**
> Use two Docker networks throughout to visualize segmentation:
>
> - `front_net` (UI ↔ API)
> - `back_net` (API ↔ workers/queues/db/iseries/ftp)
>
> Add aliases per service (e.g., `api`, `queue`, `db`) so hostnames in code are stable.

---

## Phase 00 — Hello API (orchestrator seed)

### A. Lucidchart

- Create a new doc: **“Net Refresh – Phase 00”**.
- Add a container **“App Stack”** with one node: **API**.
- Add an external **Client** shape → arrow to **API** (HTTP 8080).
- Style: green = stateless, blue = stateful, orange = infra. API = green.

### B. Docker & Code (minimal)

- Endpoint: `GET /health` → `{status:"ok"}`
- `compose/phase-00.yml`:

```yaml
services:
  api:
    image: node:22-alpine
    working_dir: /app
    command: sh -c "npm i && node server.js"
    volumes: ["../services/api:/app"]
    ports: ["8080:8080"]
    networks: [front_net, back_net]
networks: { front_net: {}, back_net: {} }
````

> **Note**: The volume path uses `../services/api:/app` because the compose file is in the `compose/` subdirectory.

**Prerequisites**: Ensure `services/api/package.json` exists with Express dependency and `"type": "module"` for ES6 imports.

**Validate:** `curl http://localhost:8080/health` → 200.

### Common Issues & Solutions

**Problem**: `npm error code ENOENT... package.json`

- **Solution**: Ensure `services/api/package.json` exists with Express dependency
- **Root Cause**: ES6 imports require proper package.json configuration

**Problem**: Volume mount not working

- **Solution**: Use correct relative path `../services/api:/app` (not `./services/api:/app`)
- **Root Cause**: Compose file is in `compose/` subdirectory

**Problem**: Permission denied removing `node_modules`

- **Solution**: Stop containers first: `docker compose -f compose/phase-00.yml down`
- **Root Cause**: Docker creates files as root user

---

## Phase 01 — Add UI (static front-end)

### A. Lucidchart

- Duplicate doc → **Phase 01**.
- Add **UI** to **App Stack**; connect **Client → UI** (HTTP 3000 or 80).
- Connect **UI → API** (HTTP). Label lines with protocol/port.

### B. Docker

- UI can be Nginx serving a single `index.html` that fetches `/api/health`.

```yaml
services:
  ui:
    image: nginx:alpine
    volumes: ["./services/ui:/usr/share/nginx/html:ro"]
    ports: ["3000:80"]
    networks: [front_net]
  api:
    # (same as Phase 00)
networks: { front_net: {}, back_net: {} }
```

**Validate:** Open `http://localhost:3000` → page shows API health.

---

## Phase 02 — Persistence (Postgres) & Cache (Redis)

### A. Lucidchart

- Add **DB (Postgres)** (blue/stateful) and **Cache (Redis)** (orange/infra) inside **App Stack**.
- Connect **API → DB** (TCP 5432), **API → Cache** (TCP 6379).
- Add **back_net** boundary around API/DB/Cache.

### B. Docker

```yaml
services:
  db:
    image: postgres:16-alpine
    environment: ["POSTGRES_PASSWORD=postgres", "POSTGRES_DB=app"]
    volumes: ["pgdata:/var/lib/postgresql/data"]
    networks: [back_net]
  cache:
    image: redis:7-alpine
    networks: [back_net]
  api:
    environment:
      DB_HOST: db
      DB_USER: postgres
      DB_PASS: postgres
      DB_NAME: app
      CACHE_HOST: cache
    depends_on: [db, cache]
volumes: { pgdata: {} }
networks: { front_net: {}, back_net: {} }
```

**API demo:** `GET /greet?name=Ana` → store “Ana” in DB; cache last greeting.

---

## Phase 03 — Introduce RabbitMQ & Worker

### A. Lucidchart

- Add **Queue (RabbitMQ)** (orange).
- Add **Worker** (green) connected to **Queue** (AMQP 5672).
- Show **API → Queue** (publish). **Worker → API or DB** as needed.

### B. Docker

```yaml
services:
  queue:
    image: rabbitmq:3-management
    ports: ["15672:15672"] # mgmt UI
    networks: [back_net]
  worker:
    image: node:22-alpine
    working_dir: /app
    command: sh -c "npm i && node worker.js"
    volumes: ["./services/worker:/app"]
    environment: { AMQP_URL: "amqp://queue" }
    depends_on: [queue]
    networks: [back_net]
  api:
    environment: { AMQP_URL: "amqp://queue" }
    depends_on: [queue]
```

**Hello-world flow:** `POST /jobs/uppercase {"text":"hello"}` → API publishes → Worker consumes → logs or writes result to DB.
**Validate:** RabbitMQ UI at `:15672` (guest/guest).

---

## Phase 04 — FTP Integration via Worker + Queue

### A. Lucidchart

- Add **FTP Server** (blue, stateful) in **back_net**.
- Flow: **API → Queue** (“ftp.upload” job) → **Worker → FTP** (21).
- Optionally **FTP → Worker** for polling new files (“ftp.import”).

### B. Docker

```yaml
services:
  ftp:
    image: fauria/vsftpd
    environment:
      - FTP_USER=user
      - FTP_PASS=pass
      - PASV_ADDRESS=ftp
    volumes: ["./services/ftp/data:/home/vsftpd"]
    networks: [back_net]
```

**Worker demo:** on `POST /files`, publish job; worker connects to `ftp:21` and uploads a tiny text file.
**Validate:** file appears under `services/ftp/data`.

---

## Phase 05 — iSeries Adapter (façade)

> We won’t spin a real IBM i/DB2. Instead, you’ll build an **iseries-adapter** microservice that exposes a very small REST contract (e.g., `/iseries/customer/:id`) and _internally_ simulates protocol/format peculiarities (fixed-width records, EBCDIC conversion stub, etc.). This isolates the legacy specifics.

### A. Lucidchart

- Add **iSeries** (external system) with a **façade: iSeries Adapter** inside back_net.
- Show **API → iSeries Adapter** (HTTP).
- Optionally **Adapter → Queue** for async calls/timeouts.

### B. Docker

```yaml
services:
  iseries-adapter:
    image: node:22-alpine
    working_dir: /app
    command: sh -c "npm i && node adapter.js"
    volumes: ["./services/iseries-adapter:/app"]
    networks: [back_net]
```

**Hello-world:** `GET /iseries/customer/42` returns a mock fixed-width translation (e.g., converts `{name:"JOSE"}` into “JOSE\_\_\_\_\_”).

---

## Phase 06 — Orchestration Patterns in API

### A. Lucidchart

- Annotate the API with two patterns:

  - **Synchronous orchestration**: UI → API → DB/Adapter → response.
  - **Asynchronous orchestration**: UI → API → Queue; Worker(s) complete; UI polls `/status/:id`.

- Layer your diagram: **Presentation**, **Orchestration**, **Async processing**, **Data/Infra**.

### B. Implementation

- Add endpoints that demonstrate both:

  - `/customer/:id/details` → fan-out to DB, Cache, iSeries Adapter; combine.
  - `/batch` → enqueue jobs; return `jobId`; add `/jobs/:id` for status.

---

## Phase 07 — Observability + Config

### A. Lucidchart

- Add **Logging** and **Metrics** sidecars or a simple **Prometheus+Grafana** box (optional).
- Add **.env** sheet to note config per service.

### B. Docker

- Minimal win: add **api** and **worker** structured logging; expose `/metrics` counters (requests, queue jobs).
- Add **healthchecks** in Compose:

```yaml
healthcheck:
  test: ["CMD", "wget", "-qO-", "http://localhost:8080/health"]
  interval: 10s
  timeout: 3s
  retries: 5
```

**Validate:** bring down a dependency and watch health degrade.

---

## Phase 08 — Resilience & Scaling

### A. Lucidchart

- Duplicate worker nodes to show horizontal scaling.
- Add **Retry** and **DLQ** (dead-letter queue) boxes beside RabbitMQ.

### B. Docker

- Scale workers: `docker compose -f compose/phase-08.yml up --scale worker=3`.
- RabbitMQ DLQ policy (declare an `*-dlq` and route nacks/timeouts).
- API retry (idempotency key) when calling iSeries Adapter.

**Exercise:** kill one worker; jobs still complete.

---

## Phase 09 — Security per Segment

### A. Lucidchart

- Shade **front_net** vs **back_net**; add **API Gateway/Reverse Proxy** (e.g., Nginx) in front of API.
- Add lock icons on lines carrying creds (DB/FTP/AMQP); note TLS options.

### B. Docker

- Put **API** behind **gateway** (publish only gateway’s port).
- Move secrets to `.env` and demonstrate a per-service env injection.
- Optional: self-signed TLS for gateway.

---

## Phase 10 — Final Composition & Runbook

### A. Lucidchart

- Produce a single **Network Composition Diagram**:

  - Swimlanes: **Client**, **UI**, **API (Orchestrator)**, **Async (Queue/Workers)**, **Data/Legacy (DB/Cache/iSeries/FTP)**, **Observability**, **Gateway**.
  - Use line notations: HTTP, AMQP, FTP, TCP:5432, etc.
  - Add callouts for: sync vs async, idempotency, DLQ, healthchecks.

- Export **PNG + PDF** to `/docs/lucid/phase-10`.

### B. Docker

- Create `compose/phase-10.yml` that includes all components and sensible depends_on/healthchecks.
- Add a `Makefile`:

```makefile
up:
	docker compose -f compose/phase-$(PHASE).yml up -d
down:
	docker compose -f compose/phase-$(PHASE).yml down
logs:
	docker compose -f compose/phase-$(PHASE).yml logs -f $(S)

# Usage examples:
# make up PHASE=00
# make logs PHASE=00 S=api
```

**Final demo script (5 minutes):**

1. Open UI; trigger sync detail fetch.
2. Trigger batch job; watch RabbitMQ UI; inspect worker logs; check `/jobs/:id`.
3. Upload via `/files`; verify on FTP.
4. Call iSeries adapter endpoint.
5. Kill a worker; show resiliency.
6. Show final diagram and explain flows.

---

## Lucidchart Tips (fast & clean)

- **Shape library:** Containers, AWS/GCP generic icons (for DB, Cache, Queue), Network → Boundary boxes.
- **Colors:** Green (stateless runtime), Blue (stateful data), Orange (infra/messaging), Gray (external).
- **Line styles:** Solid = sync HTTP; Dashed = async/AMQP; Dotted = management/metrics.
- **Layers:** One layer per phase so you can toggle complexity on/off.
- **Data fields:** Add shape data (port, protocol, env vars).
- **Callouts:** Mini notes for “retries: 3”, “timeout: 2s”, “DLQ: enabled”.

---

## Minimal code stubs (illustrative)

**API (Express)**

```js
// services/api/server.js
import express from "express";
import fetch from "node-fetch";
const app = express();
app.use(express.json());
app.get("/health", (_, res) => res.json({ status: "ok" }));
app.post("/jobs/uppercase", async (req, res) => {
  /* publish to AMQP */ res.json({ jobId: "123" });
});
app.get("/jobs/:id", (req, res) =>
  res.json({ id: req.params.id, status: "done" })
);
app.get("/customer/:id/details", async (req, res) => {
  const iser = await fetch(
    "http://iseries-adapter:8082/iseries/customer/" + req.params.id
  ).then((r) => r.json());
  res.json({ id: req.params.id, iseries: iser });
});
app.listen(8080);
```

**Worker (Node + amqplib)**

```js
// services/worker/worker.js
import amqp from "amqplib";
const conn = await amqp.connect(process.env.AMQP_URL);
const ch = await conn.createChannel();
await ch.assertQueue("uppercase");
ch.consume("uppercase", (msg) => {
  const { text } = JSON.parse(msg.content.toString());
  console.log(text.toUpperCase());
  ch.ack(msg);
});
```

**iSeries Adapter (mock)**

```js
// services/iseries-adapter/adapter.js
import express from "express";
const app = express();
app.get("/iseries/customer/:id", (req, res) => {
  const name = "JOSE"; // mock
  const fixed = name.padEnd(10, "_");
  res.json({ id: req.params.id, fixedRecord: fixed });
});
app.listen(8082);
```

---

## Checklists per Phase

- **Diagram complete & exported** (PNG/PDF saved under `/docs/lucid/phase-XX`).
- **Compose file works** (`docker compose -f compose/phase-XX.yml up -d`).
- **Hello-world validation** (explicit curl/UI step).
- **Notes** in `/docs/runbooks/phase-XX.md`: purpose, ports, envs, gotchas.

---

## Stretch goals (optional)

- Replace Nginx UI with a small Vite/React or Angular page.
- Add API Gateway route mapping (`/api`, `/metrics`), and rate-limit.
- Add Prometheus + Grafana for real metrics.
- Use **docker healthchecks** to gate worker startup on queue readiness.
- Add **OpenAPI** for API and share in Lucidchart via a link.

---

If you want, I can generate a starter repo structure (folders + Phase 00 compose + tiny API/UI stubs) so you can hit the ground running.
