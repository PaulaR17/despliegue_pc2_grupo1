# Despliegue EcoTraffic

Repositorio de despliegue para la entrega de PC2 (Universidad Europea de Madrid, grupo 1).

Levanta toda la plataforma (Postgres + Laravel + Angular) con un solo
`docker compose up`. El compose construye las imágenes de back y front
directamente desde sus repos de GitHub, así que aquí no hay código de aplicación.

## Repositorios relacionados

- Backend: <https://github.com/PaulaR17/backend_pc2_grupo1>
- Frontend: <https://github.com/PaulaR17/frontend_pc2_grupo1>
- PC1 (ETL + ML): <https://github.com/MatiuxG/Proyecto-de-Computacion-I> (no se despliega aquí)

## Requisitos en la MV

- Docker y docker compose v2

## Cómo desplegar

```bash
git clone https://github.com/PaulaR17/despliegue_pc2_grupo1.git
cd despliegue_pc2_grupo1
cp .env.sample .env
# edita .env y pon los CAMBIAME reales (APP_KEY, JWT_SECRET, ORS_API_KEY, ...)
docker compose up -d --build
```

Al primer arranque el container backend ejecuta migraciones + seeder, así que
ya hay usuarios de prueba:

| Email                    | Contraseña     | Rol   |
|--------------------------|----------------|-------|
| admin@ecotraffic.com     | password123    | ADMIN |
| usuario@ecotraffic.com   | password123    | USER  |
| paula@ecotraffic.com     | password123    | USER  |

## Acceder desde el navegador

Añade el dominio en tu `/etc/hosts` (Linux/Mac) o `C:\Windows\System32\drivers\etc\hosts` (Windows):

```
<IP de LORCA>   ecotraffic.local
```

Y abre <http://ecotraffic.local>.

## Estructura

- `docker-compose.yml` — orquesta postgres + backend + frontend.
- `nginx.conf` — se monta dentro del container frontend; sirve la SPA en `/`
  y reenvía `/api/*` al backend via FastCGI (puerto 9000).
- `.env.sample` — plantilla de variables. Copiar a `.env` y rellenar.
