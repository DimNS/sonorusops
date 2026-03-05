# SonorusOps

Etymology of the word SonorusOps: sonar + chorus + ops.

## Application docker compose example

- First application:

    ```yaml
    name: firstapp

    services:
        firstapp_db:
            image: postgres:latest
            container_name: firstapp_postgres
            networks:
                - firstapp-network
                - sonorusops-network

        firstapp_backend:
            image: ghcr.io/firstapp/backend:latest
            container_name: firstapp_backend
            labels:
                - "app.project.name=firstapp"
                - "app.service.name=api"
                - "app.service.type=backend"
            environment:
                OTEL_EXPORTER_OTLP_ENDPOINT: "http://sonorusops_jaeger:4318"
            networks:
                - firstapp-network
                - sonorusops-network

        firstapp_frontend:
            image: ghcr.io/firstapp/frontend:latest
            container_name: firstapp_frontend
            labels:
                - "app.project.name=firstapp"
                - "app.service.name=ui"
                - "app.service.type=frontend"
            networks:
                - firstapp-network
                - sonorusops-network

    networks:
        firstapp-network:
            external: true
        sonorusops-network:
            external: true
    ```

- Second application:

    ```yaml
    name: secondapp

    services:
        secondapp_db:
            image: postgres:latest
            container_name: secondapp_postgres
            networks:
                - secondapp-network
                - sonorusops-network

        secondapp_backend:
            image: ghcr.io/secondapp/backend:latest
            container_name: secondapp_backend
            labels:
                - "app.project.name=secondapp"
                - "app.service.name=api"
                - "app.service.type=backend"
            environment:
                OTEL_EXPORTER_OTLP_ENDPOINT: "http://sonorusops_jaeger:4318"
            networks:
                - secondapp-network
                - sonorusops-network

        secondapp_frontend:
            image: ghcr.io/secondapp/frontend:latest
            container_name: secondapp_frontend
            labels:
                - "app.project.name=secondapp"
                - "app.service.name=ui"
                - "app.service.type=frontend"
            networks:
                - secondapp-network
                - sonorusops-network

    networks:
        secondapp-network:
            external: true
        sonorusops-network:
            external: true
    ```

## Default ports

- `4300`: Grafana UI
- `4317`: OTLP gRPC
- `4318`: OTLP HTTP

## Start

- Clone: `git clone https://github.com/DimNS/sonorusops.git`
- Edit datasource PostgreSQL in `grafana/provisioning/datasources/firstapp-postgres.yaml`.
- Edit include containers in `vector.yaml`.
- Edit job in `victoriametrics.yaml`.
- Create network: `docker network create sonorusops-network`
- Run: `docker compose up -d`
