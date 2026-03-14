# Section 1 - Task B: Dockerize a Python Web App

## Goal
Package a basic Flask web application into a Docker image and run it with Docker Compose.

## Files
- `app.py` - simple Flask app with `/` and `/health` endpoints
- `requirements.txt` - Python dependencies
- `Dockerfile` - builds the application image
- `docker-compose.yml` - runs the container and exposes port 5000

## How to run

### Build and start
```bash
docker compose up --build