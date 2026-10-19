# Backend de Turnos y Reservas - Pre-entrega 3

API REST con Express y FileSystem que gestiona servicios y reservas. Los datos se guardan en `src/data/services.json` y `src/data/bookings.json`, por lo que se conservan al reiniciar el servidor.

## Instalación y ejecución

```bash
git clone https://github.com/Teovi13/backend-turnos-pe3.git
cd backend-turnos-pe3
npm install
cp .env.example .env
npm start
```

El servidor queda en `http://localhost:8080`.

## Variables de entorno

| Variable   | Ejemplo       |
| ---------- | ------------- |
| `PORT`     | `8080`        |
| `NODE_ENV` | `development` |

## Estructura

```
src/
  config/env.config.js
  managers/ServiceManager.js, BookingManager.js
  routes/services.router.js, bookings.router.js
  data/services.json, bookings.json
  utils/        errores y lectura/escritura de JSON compartidos
  app.js
  server.js
```

## Endpoints

| Método | Ruta                               | Descripción                               | Códigos       |
| ------ | ---------------------------------- | ----------------------------------------- | ------------- |
| GET    | `/api/services`                    | Lista servicios (`?category=`, `?available=`) | 200, 400  |
| GET    | `/api/services/:sid`               | Obtiene un servicio                       | 200, 404      |
| POST   | `/api/services`                    | Crea un servicio                          | 201, 400      |
| PUT    | `/api/services/:sid`               | Actualiza un servicio (el id no cambia)   | 200, 400, 404 |
| DELETE | `/api/services/:sid`               | Elimina un servicio                       | 200, 404      |
| POST   | `/api/bookings`                    | Crea una reserva                          | 201, 400      |
| GET    | `/api/bookings/:bid`               | Obtiene una reserva                       | 200, 404      |
| POST   | `/api/bookings/:bid/services/:sid` | Agrega un servicio a una reserva          | 200, 400, 404 |

**Servicio:** `id`, `name`, `description`, `duration`, `price`, `category`, `available`.

**Reserva:** `id`, `clientName`, `clientEmail`, `date` (`YYYY-MM-DD`), `time` (`HH:MM`), `status` (`pending` por defecto), `services`.

Reglas de las reservas:

- Se crean con `services` vacío.
- Al agregar un servicio se valida que la reserva y el servicio existan (404) y que el servicio esté disponible (400).
- Se guarda solo el id del servicio y su cantidad: `{ "service": "<id>", "quantity": 1 }`. Si el servicio ya estaba, se incrementa `quantity`.

## Ejemplo

```bash
curl -X POST http://localhost:8080/api/bookings \
  -H "Content-Type: application/json" \
  -d '{"clientName":"Ana Pérez","clientEmail":"ana@mail.com","date":"2026-11-05","time":"10:30"}'

curl -X POST http://localhost:8080/api/bookings/<bid>/services/4971a0ab-8dcf-42a8-a230-38db8e38e674

curl http://localhost:8080/api/bookings/<bid>
```
