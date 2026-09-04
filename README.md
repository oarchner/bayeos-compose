# bayeos-compose

Runtime deployment for the BayEOS V3 API stack using **Docker Hub images only**
(no local builds). Every service is pulled from Docker Hub.

## Usage

```sh
docker compose up -d
```

This pulls all images from Docker Hub and starts the full stack:

| Service          | Image                                                  |
| ---------------- | ------------------------------------------------------ |
| questdb          | `questdb/questdb:latest`                               |
| jaeger           | `jaegertracing/jaeger:2.20.0`                          |
| otel-collector   | `otel/opentelemetry-collector-contrib:0.142.0`         |
| postgresdb       | `postgres:latest`                                      |
| bayeos-api       | `${BAYEOS_API_IMAGE:-oarchner/bayeos-api:latest}`     |

The `bayeos-api` image must be published on Docker Hub. Point it at a specific
release by setting `BAYEOS_API_IMAGE` in `.env` (see the `.env` template).

## HTTP and TLS

`bayeos-api` serves plain HTTP on port `5532`. It does not terminate TLS
itself; to expose the API over HTTPS, place a reverse proxy (e.g. Caddy or
nginx) in front and terminate TLS there, forwarding to `bayeos-api:5532`.

## Config files

- `jaeger-config.yaml` and `otel-collector-config.yaml` are mounted read-only
  into the `jaeger` and `otel-collector` containers respectively.

## Data volumes

`questdb_data` and `postgresdb_data` are Docker named volumes. Remove them with:

```sh
docker compose down -v
```
