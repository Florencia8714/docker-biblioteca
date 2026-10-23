# Biblioteca API (Docker)

API REST de una biblioteca hecha con Express y MySQL, dockerizada con Docker Compose.

## Cómo levantarlo

Requisito: Docker Desktop (o Docker Engine en Linux).

```bash
cp .env.example .env
docker compose up -d --build
```

En Windows PowerShell, el primer comando es `Copy-Item .env.example .env`.

Editá el `.env` y cambiá las contraseñas de ejemplo antes de levantar.

- API: http://localhost:3000
- Salud de la API y la base: http://localhost:3000/api/salud
- Libros: http://localhost:3000/api/libros
- Base de datos (Workbench u otro cliente): 127.0.0.1:3307

Si el puerto 3000 o el 3307 están ocupados en tu compu, cambiá `BACKEND_PORT` o `DB_HOST_PORT` en el `.env`.

## Variables de entorno

| Variable | Para qué sirve |
|---|---|
| `DB_ROOT_PASSWORD` | Contraseña del usuario root de MySQL |
| `DB_NAME` | Nombre de la base (`biblioteca`) |
| `DB_USER` / `DB_PASSWORD` | Usuario y contraseña que usa el backend |
| `BACKEND_PORT` | Puerto de tu compu para la API |
| `DB_HOST_PORT` | Puerto de tu compu para MySQL |

El archivo `.env` no se sube a GitHub; `.env.example` sí.

## Cómo se comunica

```
Navegador → localhost:3000 → contenedor backend (Express, puerto 3000)
                                   → db:3306 (contenedor MySQL, red de Docker)
                                         → volumen datos-mysql
```

- **¿Qué dirección se abre en el navegador?** `http://localhost:3000`.
- **¿Cómo llega una petición al backend?** El navegador entra por el puerto 3000 de mi compu. Docker lo redirige (`ports: "3000:3000"`) al puerto 3000 del contenedor `backend`, donde escucha Express.
- **¿Con qué host y puerto se conecta el backend a MySQL?** Con `db:3306`. No usa `localhost` porque dentro del contenedor del backend `localhost` es el propio backend, donde no hay MySQL. `db` es el nombre del servicio en `compose.yaml` y Docker lo resuelve en su red interna. El puerto 3307 es solo para conectarme desde mi compu.
- **¿Dónde se guardan los datos y qué pasa con `docker compose down`?** En el volumen `datos-mysql`, que vive fuera del contenedor. `down` borra los contenedores pero no el volumen, así que los datos siguen al volver a levantar. Solo se pierden con `docker compose down -v`.

## Cómo detenerlo

```bash
docker compose down
```

No uses `-v` si querés conservar los datos.