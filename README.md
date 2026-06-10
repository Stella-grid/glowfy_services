# Spa Booking Micro-Service

Critical micro-service for the Spa & Beauty Salon appointment booking system.  
Handles appointment creation, availability checking, staff scheduling, and service management.

---

## Prerequisites

- Node.js ≥ 20
- PostgreSQL 15 (or Docker)

---

## Quick Start (Docker — recommended)

```bash
docker-compose up --build
```

The service will be available at `http://localhost:3000`.

---

## Manual Setup

```bash
# 1. Install dependencies
npm install

# 2. Configure environment
cp .env.example .env
# Edit .env with your database credentials

# 3. Start in development mode
npm run dev
```

### Environment variables

| Variable      | Default     | Description              |
|---------------|-------------|--------------------------|
| `PORT`        | `3000`      | HTTP port                |
| `DB_HOST`     | `localhost` | PostgreSQL host          |
| `DB_PORT`     | `5432`      | PostgreSQL port          |
| `DB_NAME`     | `spa_booking` | Database name          |
| `DB_USER`     | `postgres`  | Database user            |
| `DB_PASSWORD` | `password`  | Database password        |
| `NODE_ENV`    | `development` | Environment            |

---

## API Endpoints

### Health
`GET /health` — Service liveness check

### Appointments
| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/appointments` | Book a new appointment |
| `GET` | `/api/appointments` | List appointments (filters: clientId, staffId, status, from, to) |
| `GET` | `/api/appointments/:id` | Get single appointment |
| `PATCH` | `/api/appointments/:id/status` | Update appointment status |

### Services
| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/services` | List all active services |
| `POST` | `/api/services` | Create service (admin) |
| `GET` | `/api/services/:id` | Get service details |
| `PUT` | `/api/services/:id` | Update service (admin) |

### Staff
| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/staff` | List all active staff |
| `POST` | `/api/staff` | Add staff member (admin) |
| `GET` | `/api/staff/:id` | Get staff details |
| `PUT` | `/api/staff/:id/availability` | Set staff schedule (admin) |

### Availability
| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/api/availability?staffId=&date=&serviceId=` | Get available time slots |

---

## Running Tests

```bash
# Unit + integration tests (uses SQLite in-memory)
npm test

# With coverage report
npm run test:coverage
```

Tests use SQLite so no PostgreSQL instance is needed for testing.

---

## Deploying to Production

```bash
# Build Docker image
docker build -t spa-booking-service:latest .

# Run with environment variables
docker run -p 3000:3000 \
  -e DB_HOST=your-pg-host \
  -e DB_NAME=spa_booking \
  -e DB_USER=spa_user \
  -e DB_PASSWORD=your-password \
  -e NODE_ENV=production \
  spa-booking-service:latest
```

### Kubernetes (Horizontal Pod Autoscaler example)

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: booking-service-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: booking-service
  minReplicas: 2
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 70
```

---

## Static Analysis (SonarQube)

```bash
# Run SonarQube scanner (requires sonar-scanner CLI)
sonar-scanner \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.login=your-token
```

See `sonar-project.properties` for configuration.

---

## Project Structure

```
src/
├── app.js                  # Express app setup
├── server.js               # Entry point
├── models/
│   └── index.js            # Sequelize models + associations
├── controllers/
│   ├── appointmentController.js
│   ├── availabilityController.js
│   ├── serviceController.js
│   └── staffController.js
├── services/
│   └── appointmentService.js   # Core business logic
├── routes/
│   ├── appointments.js
│   ├── services.js
│   ├── staff.js
│   └── availability.js
└── middleware/
    └── errorHandler.js
tests/
└── appointment.test.js
```
# glowfy_services
