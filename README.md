# Nombre del proyecto

## Cómo levantarlo

Requisito: Docker Desktop (o Docker Engine en Linux).

```bash
cp .env.example .env
docker compose up -d --build
```

- API: http://localhost:3000
- Base de datos (Workbench): 127.0.0.1:3307

## Cómo se comunica

Navegador → backend (puerto 3000) → MySQL (`db:3306`, por la red de Docker).
Los datos se guardan en el volumen `datos-mysql`.

## Cómo detenerlo

```bash
docker compose down
```
