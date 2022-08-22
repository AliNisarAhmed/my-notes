# Docker Healthchecks

Healthchecks are supported in Dockerfile, Compose YAML, docker run, and Swarm services
- Health check commands can be specified which Docker engine can be configured to execute

Very highly recommended to have healthchecks in prod

Docker engine expects `exit 0` (OK) or `exit 1` (Error) to be returned by the healthcheck command (no other exit codes are accepted)

The container can be in three states during healthchecks
1. starting
2. healthy
3. unhealthy

Services will replace tasks if they fail healthcheck.
Service updates wait for healthchecks before continuing.

##### Example command

`docker run \
    --health-cmd="curl -f localhost:9200/_cluster/health || false" \
    --health-interval=5s \
    --health-retries=3 \
    --health-timeout=2s \
    --health-start-period=15s \
    elasticsearch:2`

Dockerfile
- Options for healthcheck command
    - --interval=DURATION (default: 30s)
    - --timeout=DURATION (default: 30s)
    - --start-period=DURATION (default: 0s)
    - --retries=N (default: 3)
- Directive: `HEALTHCHECK curl -f http://localhost/ || exit 1`
- Custom options with the command
    - `HEALTHCHECK --timeout=2s --interval=3s --retries=3 CMD curl -f ...`

Health check in postgres
```docker
FROM postgres

HEALTHCHECK --interval=5s --timeout=3s \
    CMD pg_isready -U postgres || exit 1
```

HealthCheck in compose/stack files
```yaml
version: "3.9"

services: 
    web: 
        image: nginx
        healthcheck: 
            test: ["CMD", "curl", "-f", "http://localhost"]
            interval: 1m30s
            timeout: 10s
            retries: 3
            start_period: 1m
```