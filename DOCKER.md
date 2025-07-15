# Docker Setup for Chat Application

This document explains how to run the Chat Application using Docker and Docker Compose.

## Prerequisites

- Docker installed on your system
- Docker Compose installed on your system

## Quick Start

1. **Clone the repository:**
   ```bash
   git clone https://github.com/akashdeep023/Chat_App.git
   cd Chat_App
   ```

2. **Build and run with Docker Compose:**
   ```bash
   docker-compose up --build
   ```

3. **Access the application:**
   - Frontend: http://localhost:3000
   - Backend API: http://localhost:9000
   - MongoDB: localhost:27017

## Docker Services

### 1. MongoDB (Database)
- **Image:** mongo:6.0
- **Port:** 27017
- **Data persistence:** Uses Docker volume `mongodb_data`

### 2. Backend (Node.js/Express)
- **Build:** Custom Dockerfile in `./backend`
- **Port:** 9000
- **Environment:** Production mode with MongoDB connection

### 3. Frontend (React/Vite)
- **Build:** Multi-stage Dockerfile with Nginx
- **Port:** 3000 (mapped to Nginx port 80)
- **Features:** Includes proxy configuration for API and Socket.IO

## Environment Variables

### Backend (.env.docker)
```
NODE_ENV=production
PORT=9000
MONGODB_URI=mongodb://mongodb:27017/chat-app
JWT_SECRET=secret-kvndkvdlkajkhkJkBiu6JJNjkbhkvnskcmhLJ5dKbkjsamnv
FRONTEND_URL=http://localhost:3000
```

### Frontend (.env.docker)
```
VITE_BACKEND_URL=http://localhost:9000
```

## Docker Commands

### Development
```bash
# Build and start all services
docker-compose up --build

# Start services in background
docker-compose up -d

# View logs
docker-compose logs

# Stop services
docker-compose down

# Remove volumes (careful: this deletes database data)
docker-compose down -v
```

### Individual Services
```bash
# Build backend only
docker build -t chat-app-backend ./backend

# Build frontend only
docker build -t chat-app-frontend ./frontend

# Run backend container
docker run -p 9000:9000 chat-app-backend

# Run frontend container
docker run -p 3000:80 chat-app-frontend
```

## Production Deployment

For production deployment, consider:

1. **Environment Variables:** Update the environment files with production values
2. **Security:** Change JWT_SECRET to a secure random string
3. **Database:** Use a managed MongoDB service or persistent volumes
4. **Reverse Proxy:** Use a reverse proxy like Nginx or Traefik
5. **SSL/TLS:** Configure HTTPS certificates

## Troubleshooting

1. **Port conflicts:** Make sure ports 3000, 9000, and 27017 are available
2. **MongoDB connection:** Ensure MongoDB container is running before backend
3. **Build issues:** Clear Docker cache with `docker system prune`
4. **Socket.IO issues:** Check if the proxy configuration in nginx.conf is correct

## File Structure

```
Chat_App/
├── docker-compose.yml
├── backend/
│   ├── Dockerfile
│   ├── .dockerignore
│   └── .env.docker
├── frontend/
│   ├── Dockerfile
│   ├── .dockerignore
│   ├── .env.docker
│   └── nginx.conf
└── DOCKER.md
```
