# Chat Application - Docker Deployment

This project contains a chat application with a React frontend and Node.js backend, ready for Docker deployment.

## Architecture

- **Frontend**: React app served by Nginx on port 80
- **Backend**: Node.js/Express API on port 3000
- **Database**: MongoDB on port 27017

## Quick Start

### Local Development
```bash
docker-compose up --build
```

### Production Deployment
```bash
docker-compose -f docker-compose.prod.yml up --build -d
```

## Services

### Frontend (Port 80)
- React application built with Vite
- Served by Nginx
- Accessible at `http://localhost`

### Backend (Port 3000)
- Node.js Express API
- Socket.IO for real-time communication
- Accessible at `http://localhost:3000`

### Database (Port 27017)
- MongoDB database
- Persistent data storage with Docker volumes

## Environment Configuration

### Backend Environment Variables
Update `backend/.env.production` with your production values:

```env
PORT=3000
NODE_ENV=production
FRONTEND_URL=http://your-domain.com
MONGODB_URI=mongodb://admin:password@mongo:27017/chatapp?authSource=admin
JWT_SECRET=your-super-secret-jwt-key-here
```

### Frontend Environment Variables
Update `frontend/.env.production` with your production values:

```env
VITE_API_BASE_URL=http://your-domain.com:3000
VITE_SOCKET_URL=http://your-domain.com:3000
```

## Cloud Deployment Steps

### 1. Prepare for Cloud Deployment
- Update environment variables in `.env.production` files
- Change MongoDB credentials
- Set strong JWT secret
- Update CORS origins in backend

### 2. Deploy to Cloud Provider

#### For AWS EC2/DigitalOcean/VPS:
```bash
# Clone repository
git clone <your-repo-url>
cd Cloud_Deploy

# Update environment variables
nano backend/.env.production
nano frontend/.env.production

# Deploy
docker-compose -f docker-compose.prod.yml up --build -d
```

#### For Cloud Platforms (Heroku, Railway, etc.):
- Use the individual Dockerfiles in each service directory
- Set environment variables in your cloud platform's dashboard
- Deploy frontend and backend as separate services

### 3. Domain Configuration
- Point your domain to the server IP
- Update `FRONTEND_URL` in backend environment
- Update API URLs in frontend environment
- Configure SSL/TLS certificates (recommended)

## Commands

### Build and run all services
```bash
docker-compose up --build
```

### Run in background (production)
```bash
docker-compose -f docker-compose.prod.yml up --build -d
```

### Stop all services
```bash
docker-compose down
```

### View logs
```bash
docker-compose logs -f [service-name]
```

### Rebuild specific service
```bash
docker-compose build [service-name]
```

## File Structure

```
├── backend/
│   ├── Dockerfile
│   ├── .dockerignore
│   ├── .env.production
│   └── ... (backend files)
├── frontend/
│   ├── Dockerfile
│   ├── .dockerignore
│   ├── .env.production
│   ├── nginx.conf
│   └── ... (frontend files)
├── docker-compose.yml (development)
├── docker-compose.prod.yml (production)
└── README.md
```

## Security Notes

1. **Change default MongoDB credentials** in production
2. **Use strong JWT secret** (generate a random string)
3. **Configure CORS properly** for your domain
4. **Use HTTPS** in production
5. **Set up firewall rules** to protect your server
6. **Regular backups** of MongoDB data

## Troubleshooting

### Common Issues

1. **Port conflicts**: Make sure ports 80, 3000, and 27017 are available
2. **Environment variables**: Check that all required environment variables are set
3. **Network issues**: Ensure services can communicate within the Docker network
4. **Build failures**: Check Dockerfile syntax and dependencies

### Debugging Commands

```bash
# Check running containers
docker ps

# View container logs
docker logs <container-name>

# Access container shell
docker exec -it <container-name> /bin/sh

# Check network connectivity
docker network ls
docker network inspect cloud_deploy_app-network
```

## Production Checklist

- [ ] Update MongoDB credentials
- [ ] Set strong JWT secret
- [ ] Configure proper CORS origins
- [ ] Set production API URLs
- [ ] Configure SSL/TLS
- [ ] Set up monitoring
- [ ] Configure backups
- [ ] Test all functionality
