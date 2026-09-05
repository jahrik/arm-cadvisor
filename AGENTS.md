# AGENTS.md

Multi-arch cAdvisor image: a pinned `FROM` over official `gcr.io/cadvisor/cadvisor`, part of the `monitor` swarm stack.

## Commands

```bash
just build                                  # build jahrik/arm-cadvisor:latest
curl -fsS http://localhost:8080/healthz     # smoke test a running container
just deploy                                 # swarm stack deploy (stack: monitor)
```

## CI

`build.yml`: Test (build + `/healthz`) on PR; Release (buildx amd64+arm64+armv7 push to Docker Hub) on merge to main. Needs `DOCKERHUB_USERNAME`/`DOCKERHUB_TOKEN` secrets.

## Quirks

- Upstream image tags lag GitHub releases — check `gcr.io/v2/cadvisor/cadvisor/tags/list` before bumping `FROM`.
- `docker-compose.yml` is a swarm fragment: `mode: global`, external `monitor` overlay network — keep that wiring.
- Host mounts are required for real metrics; `/healthz` works without them.
