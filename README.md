WebBitcoinEscrow
Minimalist and secure web based escrow service for Bitcoin powered by blockchain.info written in Go.

Main Features!
Very secure due to secret based system rather than using authentication
Written in Go thus highly performant and low in RAM usage
Easy to deploy
Adminpanel to deal with escalated escrows.

 - - -

 # WebBitcoinEscrow Deployment Guide

This guide will help you set up GitHub Actions for your Bitcoin escrow application with multiple deployment options.

## 🚀 Quick Setup

### 1. Add Files to Your Repository

Add these files to your repository:
- `.github/workflows/deploy.yml` (the GitHub Actions workflow)
- `Dockerfile` (for containerization)
- `docker-compose.yml` (for local development)

### 2. Configure GitHub Secrets

Go to your GitHub repository → Settings → Secrets and variables → Actions, then add:

#### Required Secrets:
```
DOCKER_USERNAME          # Your Docker Hub username
DOCKER_PASSWORD          # Your Docker Hub password/token
```

#### For VPS Deployment (Optional):
```
VPS_HOST                 # Your VPS IP address
VPS_USERNAME             # SSH username (usually 'root' or 'ubuntu')
VPS_SSH_KEY             # Your private SSH key
VPS_PORT                # SSH port (usually 22)
```

#### For Railway Deployment (Optional):
```
RAILWAY_TOKEN           # Your Railway API token
```

#### Application Environment Variables:
```
DATABASE_URL            # Database connection string
BLOCKCHAIN_API_KEY      # blockchain.info API key
ADMIN_SECRET           # Secret key for admin access
```

## 🛠 Deployment Options

### Option 1: VPS Deployment (Recommended)

1. **Set up a VPS** (DigitalOcean, Linode, etc.)
2. **Install Docker** on your VPS:
   ```bash
   curl -fsSL https://get.docker.com -o get-docker.sh
   sudo sh get-docker.sh
   sudo usermod -aG docker $USER
   ```
3. **Configure SSH access** and add your SSH key to GitHub secrets
4. **Push to main branch** - the workflow will automatically deploy

### Option 2: Railway Deployment

1. **Sign up for Railway** at railway.app
2. **Get your API token** from Railway dashboard
3. **Add token to GitHub secrets**
4. **Push to main branch**

### Option 3: Manual Docker Deployment

1. **Build the image locally**:
   ```bash
   docker build -t web-bitcoin-escrow .
   ```

2. **Run with Docker Compose**:
   ```bash
   docker-compose up -d
   ```

3. **Or run directly**:
   ```bash
   docker run -d \
     --name web-bitcoin-escrow \
     -p 8080:8080 \
     -e DATABASE_URL="your-db-url" \
     -e BLOCKCHAIN_API_KEY="your-api-key" \
     -e ADMIN_SECRET="your-secret" \
     web-bitcoin-escrow
   ```

## 🔧 Configuration

### Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `PORT` | Server port (default: 8080) | No |
| `DATABASE_URL` | Database connection string | Yes |
| `BLOCKCHAIN_API_KEY` | blockchain.info API key | Yes |
| `ADMIN_SECRET` | Admin panel secret key | Yes |
| `DEBUG` | Enable debug mode | No |

### Database Setup

The application supports multiple databases:
- **SQLite** (for development): `sqlite:///data/escrow.db`
- **PostgreSQL** (recommended): `postgres://user:pass@host:port/dbname`
- **MySQL**: `mysql://user:pass@host:port/dbname`

## 📋 Workflow Features

The GitHub Actions workflow includes:

- ✅ **Automated Testing** - Runs tests on every push/PR
- ✅ **Multi-platform Builds** - Builds for Linux, Windows, macOS
- ✅ **Docker Images** - Automatically builds and pushes to Docker Hub
- ✅ **VPS Deployment** - Deploys to your server via SSH
- ✅ **Railway Integration** - One-click deployment to Railway
- ✅ **Release Management** - Creates releases with binaries for tags

## 🔒 Security Considerations

1. **Use strong secrets** for `ADMIN_SECRET`
2. **Rotate SSH keys** regularly
3. **Use encrypted connections** for database
4. **Keep dependencies updated**
5. **Monitor logs** for suspicious activity

## 🐛 Troubleshooting

### Common Issues:

1. **Build fails**: Check Go version and dependencies
2. **Docker push fails**: Verify Docker Hub credentials
3. **VPS deployment fails**: Check SSH connection and Docker installation
4. **App won't start**: Verify environment variables and database connection

### Debugging:

```bash
# Check container logs
docker logs web-bitcoin-escrow

# Connect to container
docker exec -it web-bitcoin-escrow sh

# Check GitHub Actions logs
# Go to Actions tab in your repository
```

## 📚 Next Steps

1. **Test the deployment** with a small transaction
2. **Set up monitoring** (consider using services like UptimeRobot)
3. **Configure backups** for your database
4. **Set up SSL/TLS** with Let's Encrypt or Cloudflare
5. **Add additional security measures** as needed

## 🆘 Support

If you encounter issues:
1. Check the GitHub Actions logs
2. Review container logs
3. Verify all environment variables are set
4. Ensure your VPS has sufficient resources

Remember to test thoroughly before handling real Bitcoin transactions!
