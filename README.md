# Full Stack Application with Docker

A modern full-stack application using NextJS, NestJS, and PostgreSQL, all containerized with Docker.

## Tech Stack

- Frontend: NextJS
- Backend: NestJS
- Database: PostgreSQL
- Containerization: Docker

## Prerequisites

- Docker (version 20.10.0 or higher)
- Docker Compose (version 2.0.0 or higher)
- Git

## Quick Start Guide

### 1. Clone and Setup

```bash
# Clone the repository
git clone [your-repository-url]
cd [repository-name]

# Create network (first time only)
docker network create app-network
```

### 2. Environment Setup

Create `.env` file in the root directory:

```env
# PostgreSQL
POSTGRES_DB=myapp_db
POSTGRES_USER=postgres
POSTGRES_PASSWORD=your_secure_password
DATABASE_URL=postgresql://${POSTGRES_USER}:${POSTGRES_PASSWORD}@postgres:5432/${POSTGRES_DB}

# Backend
BACKEND_PORT=3001

# Frontend
FRONTEND_PORT=3000
NEXT_PUBLIC_API_URL=http://localhost:3001
```

### 3. Build and Start Services

```bash
# Build all containers
docker-compose build

# Start all services
docker-compose up -d
```

### 4. Verify Installation

```bash
# Check if all containers are running
docker-compose ps

# Should show three containers running:
# - myapp-frontend
# - myapp-backend
# - myapp-postgres
```

### 5. Access Applications

- Frontend: http://localhost:3000
- Backend API: http://localhost:3001
- Database: localhost:5432 (accessible via database client)

## Common Docker Commands

### Service Management
```bash
# Start all services
docker-compose up -d

# Stop all services
docker-compose down

# Rebuild and start services
docker-compose up -d --build

# Restart a specific service
docker-compose restart [frontend|backend|postgres]
```

### Logs and Debugging
```bash
# View logs from all services
docker-compose logs -f

# View logs from specific service
docker-compose logs -f [frontend|backend|postgres]

# Check container status
docker-compose ps
```

### Database Operations
```bash
# Access PostgreSQL CLI
docker-compose exec postgres psql -U postgres -d myapp_db

# Backup database
docker-compose exec postgres pg_dump -U postgres myapp_db > backup.sql

# Reset database (Warning: destroys all data)
docker-compose down
rm -rf postgres-data
docker-compose up -d
```

## Development Workflow

### Hot Reloading
- Frontend and backend code changes will automatically reload
- Changes to `docker-compose.override.yml` require service restart
- Database changes persist in `postgres-data/` directory

### Adding Dependencies
```bash
# For frontend
docker-compose exec frontend npm install [package-name]

# For backend
docker-compose exec backend npm install [package-name]
```

## Project Structure
```
.
├── frontend/          # NextJS frontend application
│   ├── Dockerfile    # Frontend container configuration
│   └── ...
├── backend/          # NestJS backend application
│   ├── Dockerfile    # Backend container configuration
│   └── ...
├── postgres-data/    # PostgreSQL data (git-ignored)
├── .env             # Environment variables (git-ignored)
└── docker-compose.override.yml  # Docker services configuration
```

## Troubleshooting

### Common Issues

1. **Containers won't start**
```bash
# Remove all containers and volumes
docker-compose down -v

# Rebuild from scratch
docker-compose up -d --build
```

2. **Database connection issues**
```bash
# Check if postgres is running
docker-compose ps postgres

# Check postgres logs
docker-compose logs postgres
```

3. **Port conflicts**
- Ensure ports 3000, 3001, and 5432 are not in use
- Modify ports in `.env` if needed

## Important Notes

- Never commit `.env` or `postgres-data/` to version control
- Database `synchronize: true` is enabled in development - disable for production
- All services communicate through Docker network 'app-network'
- For production deployment, modify environment variables and disable development features

## Support

For issues and feature requests, please create an issue in the repository.
