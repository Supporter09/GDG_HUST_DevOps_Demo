# DevOps Training Demo

A simple Flask application demonstrating Docker, Docker Compose, and CI/CD concepts.

## 📋 Prerequisites

- Docker & Docker Compose
- GitHub account
- Docker Hub account
- Digital Ocean droplet (for deployment)

## 🚀 Quick Start

### Example 1: Simple Docker Container

Build and run the Flask app:

```bash
docker build -t devops-demo .
docker run -p 8000:8000 devops-demo
```

Visit: `http://localhost:8000`

### Example 2: Multi-Service with Docker Compose

Run the complete stack (Flask + Redis + PostgreSQL + Nginx):

```bash
docker-compose up -d
```

Services:

- **Flask API**: http://localhost:8000
- **Nginx Proxy**: http://localhost:80
- **Redis**: localhost:6379
- **PostgreSQL**: localhost:5432

Stop all services:

```bash
docker-compose down
```

## 🔧 Configuration

### GitHub Secrets (for CI/CD)

Set these in your GitHub repository settings:

| Secret            | Description                        |
| ----------------- | ---------------------------------- |
| `DOCKER_USERNAME` | Your Docker Hub username           |
| `DOCKER_PASSWORD` | Your Docker Hub password/token     |
| `DROPLET_IP`      | Digital Ocean droplet IP address   |
| `DROPLET_USER`    | SSH user (usually `root`)          |
| `DROPLET_SSH_KEY` | Private SSH key for droplet access |

### Workflow File

Edit `.github/workflows/deploy.yml` and change:

- `DOCKER_IMAGE: yourusername/devops-training` to your Docker Hub repo

## 📦 Project Structure

```
.
├── main.py                 # Flask application
├── requirement.txt         # Python dependencies
├── Dockerfile             # Docker build instructions
├── docker-compose.yaml    # Multi-service orchestration
├── nginx.conf            # Nginx configuration
├── .gitignore            # Git ignore rules
└── .github/
    └── workflows/
        └── deploy.yml    # CI/CD pipeline
```

## 🔄 CI/CD Pipeline

The pipeline automatically:

1. ✅ Checks out code
2. 🔨 Builds Docker image
3. 📤 Pushes to Docker Hub
4. 🚀 Deploys to Digital Ocean

Triggers on push to `main` branch.

## 🧪 API Endpoints

- `GET /` - Welcome message with environment info
- `GET /health` - Health check endpoint

## 📚 Learning Points

### Docker Concepts

- Multi-stage builds
- Layer caching
- Port mapping
- Environment variables

### Docker Compose

- Service orchestration
- Networks & volumes
- Service dependencies
- Multiple containers

### CI/CD

- Automated builds
- Container registry
- SSH deployment
- Secrets management

## 🛠️ Troubleshooting

**Container won't start?**

```bash
docker logs devops-app
```

**Port already in use?**

```bash
docker ps
docker stop <container-id>
```

**Clean everything:**

```bash
docker-compose down -v
docker system prune -a
```

## 📖 Next Steps

1. Fork this repository
2. Set up GitHub secrets
3. Update `DOCKER_IMAGE` in workflow
4. Push to `main` branch
5. Watch the CI/CD magic happen! ✨
