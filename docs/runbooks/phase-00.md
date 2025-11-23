# Phase 00 Runbook - Hello API

## Purpose

Establish the foundational API service with network segmentation for the microservices architecture refresh project.

## Services

- **API**: Simple Express.js server with health endpoint
- **Networks**: `front_net` and `back_net` (prepared for future phases)

## Ports

- `8080`: API HTTP endpoint

## Environment Variables

- None required for Phase 00 (all defaults work)

## Quick Start

```bash
# Start the API
docker compose -f compose/phase-00.yml up -d

# Test the API
curl http://localhost:8080/health
# Expected: {"status":"ok"}

# Stop the API
docker compose -f compose/phase-00.yml down
```

## Troubleshooting

### API won't start

1. **Check if port 8080 is available**: `lsof -i :8080`
2. **Verify package.json exists**: `ls services/api/package.json`
3. **Check Docker logs**: `docker compose -f compose/phase-00.yml logs api`

### Health endpoint returns error

1. **Verify API is running**: `docker compose -f compose/phase-00.yml ps`
2. **Check if networks are created**: `docker network ls | grep compose`
3. **Test from inside container**: `docker compose -f compose/phase-00.yml exec api wget -qO- http://localhost:8080/health`

### Permission errors with node_modules

1. **Stop containers first**: `docker compose -f compose/phase-00.yml down`
2. **Use sudo to remove**: `sudo rm -rf services/api/node_modules`
3. **Files are automatically recreated on next startup**

## Network Architecture

```
Client (external) → API (front_net + back_net) → [Future services in back_net]
```

## Files Created

- `compose/phase-00.yml`: Docker Compose configuration
- `services/api/server.js`: Express API server
- `services/api/package.json`: Node.js dependencies
- `docs/lucid/Net_Refresh_Phase_00.png`: Architecture diagram

## Next Steps

- Phase 01: Add UI service to `front_net`
- Phase 02: Add database and cache to `back_net`

## Success Criteria

- ✅ API responds to health check
- ✅ Both networks are created
- ✅ Architecture diagram exported
- ✅ All documentation complete
