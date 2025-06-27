# Deploy Command

Commands for building and deploying the application.

## Build for Production
```bash
# Build frontend
cd frontend && npm run build

# Collect static files (Django)
cd backend && python manage.py collectstatic --noinput
```

## Docker Deployment
```bash
# Build Docker image
docker build -t ribbit-app .

# Run with Docker
docker run -p 8000:8000 -it ribbit-app

# Or use the pre-built image
docker pull ghcr.io/juliantjg/ribbit-full-stack:latest
docker run -p 8000:8000 -it ghcr.io/juliantjg/ribbit-full-stack:latest
```

## Database Migration (Production)
```bash
cd backend
python manage.py migrate --run-syncdb
python manage.py createsuperuser  # If needed
```