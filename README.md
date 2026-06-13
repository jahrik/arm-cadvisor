# arm-cadvisor

[![Build](https://github.com/jahrik/arm-cadvisor/actions/workflows/build.yml/badge.svg)](https://github.com/jahrik/arm-cadvisor/actions/workflows/build.yml)

Multi-arch [cAdvisor](https://github.com/google/cadvisor) image for per-node container metrics in the `monitor` swarm stack. Built for a 2018 Pi swarm cluster; now a pinned layer over the official `gcr.io/cadvisor/cadvisor` image (whose published tags lag GitHub releases — pin what gcr.io actually has).

## Run

```bash
docker run -d --name cadvisor -p 8080:8080 \
  -v /:/rootfs:ro -v /var/run:/var/run:rw \
  -v /sys:/sys:ro -v /var/lib/docker/:/var/lib/docker:ro \
  jahrik/arm-cadvisor:latest
curl http://localhost:8080/healthz
```

## Deploy (swarm)

```bash
docker network create -d overlay monitor   # once
make deploy
```

## Build

```bash
make build
make push
```

CI: PR builds + health check; merge to main pushes multi-arch (amd64/arm64/armv7) to Docker Hub.
