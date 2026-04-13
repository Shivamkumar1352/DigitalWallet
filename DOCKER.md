# Docker Run Guide

This repository can now be started as a full Docker Compose stack.

## What gets started

- `eureka-server`
- `api-gateway`
- `auth-service`
- `user-service`
- `wallet-service`
- `reward-service`
- `notification-service`
- Kafka, Zookeeper, Redis, Zipkin
- One PostgreSQL database per stateful service
- Prometheus, Grafana, SonarQube

## Environment injection

`docker-compose.yml` loads your existing root `.env` file for secrets and overrides the infrastructure hostnames internally so containers talk to:

- PostgreSQL containers instead of `localhost`
- `redis`
- `kafka:29092`
- `eureka-server`
- `zipkin`

You do not need to manually rewrite `.env` for Docker.

## Run

```bash
docker compose up --build
```

Run detached:

```bash
docker compose up --build -d
```

Stop everything:

```bash
docker compose down
```

Stop and remove volumes too:

```bash
docker compose down -v
```

## Main endpoints

- API Gateway: `http://localhost:8080`
- Eureka: `http://localhost:8761`
- Zipkin: `http://localhost:9411`
- Prometheus: `http://localhost:9090`
- Grafana: `http://localhost:3000`
- SonarQube: `http://localhost:9000`
