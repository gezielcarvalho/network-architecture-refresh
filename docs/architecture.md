# Current architecture

## Implemented Phase 00

```text
Client
  |
  | HTTP :8080
  v
Node / Express API
  |             |
  v             v
front_net    back_net
```

The API offers `GET /health`. The Compose file runs it in a Node container and attaches it to both networks, establishing the segmentation pattern used by later planned phases.

## Roadmap boundary

The README describes a sequence that could add UI, persistence, cache, RabbitMQ/workers, FTP, an iSeries adapter, observability, resilience, and security. Those descriptions are architectural exercises and examples, not code currently present in the repository.

See the [Phase 00 runbook](runbooks/phase-00.md) and the [diagram export](lucid/Net_Refresh_Phase_00.png) for the implemented exercise.
