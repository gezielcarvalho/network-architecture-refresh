# Phase 00 – Step 2 Guide: Minimal API with Docker

This guide explains how the minimal API was set up and how to run it using Docker Compose.

## What Was Done

1. **Created Docker Compose file** (`compose/phase-00.yml`):

   - Defines an `api` service using the official Node.js 22 Alpine image.
   - Mounts the local `services/api` folder into the container.
   - Exposes port 8080.
   - Connects the service to two networks: `front_net` and `back_net`.

2. **Created API code** (`services/api/server.js`):
   - Simple Express server with a single endpoint:
     - `GET /health` returns `{status: "ok"}`.

## How to Run

1. **Start the API service:**
   ```bash
   docker compose -f compose/phase-00.yml up --build
   ```
2. **Test the API endpoint:**
   ```bash
   curl http://localhost:8080/health
   ```
   - You should see: `{"status":"ok"}`

## Troubleshooting

- If you get a connection error, make sure Docker Desktop is running and port 8080 is not in use.
- If you see missing module errors, ensure your `services/api/server.js` uses only built-in or installed modules.

---

Once you see the health check response, phase 00 is complete and running. You can now commit this step and proceed to the next phase.
