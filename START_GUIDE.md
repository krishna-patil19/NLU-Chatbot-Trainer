# 🚀 NLU ML Platform - Complete Startup Guide

## Prerequisites

Before starting, make sure you have:

- ✅ **Docker & Docker Compose** installed ([Get Docker](https://docs.docker.com/get-docker/))
- ✅ **Node.js 18+** installed ([Get Node.js](https://nodejs.org/))
- ✅ **npm or bun** package manager

---

## 🎯 Quick Start (All Services)

### Option 1: One Command Startup (Recommended)

```bash
npm run start:all
```

This will:
1. Start the Python ML Backend (Docker containers)
2. Start the Next.js Frontend (port 3000)
3. Display status of both services

### Option 2: Manual Startup

#### Step 1: Start Python ML Backend

```bash
cd python-backend
./start.sh
```

**What this does:**
- Builds Docker images for ML Service (port 8000) and Rasa NLU (port 8001)
- Starts both services in detached mode
- Runs health checks to verify services are running

**Expected Output:**
```
✅ ML Service is running at http://localhost:8000
✅ Rasa NLU Service is running at http://localhost:8001
🎉 Backend services are ready!
```

#### Step 2: Start Next.js Frontend (New Terminal)

```bash
# From project root
npm install  # First time only
npm run dev
```

**Expected Output:**
```
▲ Next.js 15.3.5
- Local:        http://localhost:3000
✓ Ready in 2.5s
```

---

## 🔍 Verify Everything is Running

### Check Backend Services

```bash
# Check ML Service
curl http://localhost:8000/health

# Check Rasa Service
curl http://localhost:8001/health
```

### Check Frontend
Open browser: **http://localhost:3000**

You should see:
- ✅ Homepage loads
- ✅ "Python ML Backend: Connected" status in dashboard (green badge)
- ✅ Training module shows "ML Service: Active"

---

## 📊 Service Ports

| Service | Port | URL | Purpose |
|---------|------|-----|---------|
| **Next.js Frontend** | 3000 | http://localhost:3000 | Main web interface |
| **Python ML Service** | 8000 | http://localhost:8000 | ML training & predictions |
| **Rasa NLU Service** | 8001 | http://localhost:8001 | NLU chatbot training |

---

## 🛑 Stopping Services

### Stop All Services

```bash
npm run stop:all
```

### Stop Individual Services

**Python Backend:**
```bash
cd python-backend
docker-compose down
```

**Next.js Frontend:**
```bash
# Press Ctrl+C in the terminal running npm run dev
```

---

## 📝 View Logs

### Python Backend Logs

```bash
cd python-backend
docker-compose logs -f
```

**To view specific service:**
```bash
docker-compose logs -f ml-service
docker-compose logs -f rasa-service
```

### Next.js Logs
Logs appear directly in the terminal where you ran `npm run dev`

---

## 🔧 Troubleshooting

### ❌ Python Backend Won't Start

**Issue:** `docker-compose: command not found`

**Solution:**
```bash
# Install Docker Compose
# macOS/Linux:
sudo apt-get install docker-compose  # Ubuntu/Debian
brew install docker-compose          # macOS

# Or use Docker Desktop which includes Compose
```

---

**Issue:** `Port 8000 or 8001 already in use`

**Solution:**
```bash
# Find process using the port
lsof -i :8000
lsof -i :8001

# Kill the process
kill -9 <PID>

# Or use different ports in python-backend/docker-compose.yml
```

---

**Issue:** `Error: Cannot connect to Docker daemon`

**Solution:**
```bash
# Start Docker Desktop
# Or start Docker daemon:
sudo systemctl start docker  # Linux
open -a Docker              # macOS
```

---

### ❌ Frontend Shows "Backend Not Connected"

**Issue:** Red badge showing "Python ML Backend: Disconnected"

**Solution:**
```bash
# 1. Check if backend is running
curl http://localhost:8000/health

# 2. If not, start backend
cd python-backend
./start.sh

# 3. Refresh frontend page
```

---

**Issue:** `EADDRINUSE: address already in use :::3000`

**Solution:**
```bash
# Find process using port 3000
lsof -i :3000

# Kill it
kill -9 <PID>

# Or use different port
npm run dev -- -p 3001
```

---

### ❌ Docker Build Fails

**Issue:** `Cannot pull Docker images` or build errors

**Solution:**
```bash
# 1. Check Docker is running
docker ps

# 2. Pull images manually
cd python-backend
docker-compose pull

# 3. Rebuild from scratch
docker-compose build --no-cache

# 4. Start services
docker-compose up -d
```

---

### ❌ Training Still Shows "Simulation Mode"

**Issue:** Training button disabled or showing simulation warning

**Solution:**
```bash
# 1. Verify backend health
curl http://localhost:8000/health

# 2. Check backend logs
cd python-backend
docker-compose logs ml-service

# 3. Restart backend
docker-compose restart

# 4. Wait 10 seconds, then refresh frontend
```

---

## 🔄 Development Workflow

### Daily Startup Routine

```bash
# 1. Start all services
npm run start:all

# 2. Open browser to http://localhost:3000

# 3. Start coding! Hot reload is enabled on both services
```

### Making Changes

**Frontend Changes:**
- Edit files in `src/`
- Changes auto-reload in browser
- No restart needed

**Backend Changes:**
- Edit files in `python-backend/`
- Restart backend: `cd python-backend && docker-compose restart`
- Or rebuild: `docker-compose up -d --build`

---

## 📦 First Time Setup

If this is your first time running the app:

```bash
# 1. Install Node.js dependencies
npm install

# 2. Create .env file
cp .env.example .env

# 3. Add your database credentials to .env
# TURSO_CONNECTION_URL=...
# TURSO_AUTH_TOKEN=...

# 4. Run database migrations
npm run db:push

# 5. Start all services
npm run start:all

# 6. Register your first account at http://localhost:3000/register
```

---

## 🎓 Usage Flow

Once everything is running:

1. **Register/Login** → http://localhost:3000/register
2. **Create Workspace** → Dashboard → "Create New Workspace"
3. **Upload Dataset** → Select workspace → Upload CSV file
4. **Train Model** → Select algorithms → Click "Train" (Backend must be running!)
5. **View Results** → See accuracy, metrics, confusion matrix
6. **Test Chatbot** → NLU Chatbot tab → Chat with your trained model
7. **Download Model** → Export as .pickle or .h5 file

---

## 💡 Tips

- **Auto-start on Boot:** Add startup script to your system's startup applications
- **Multiple Workspaces:** You can create unlimited workspaces for different projects
- **Large Datasets:** Backend handles datasets up to 500MB (configurable in Docker)
- **GPU Support:** Edit `docker-compose.yml` to enable GPU acceleration for training
- **Production:** Use `npm run build && npm start` for production mode

---

## 🆘 Still Having Issues?

1. **Check all prerequisites are installed**
2. **Verify ports 3000, 8000, 8001 are free**
3. **Check Docker Desktop is running**
4. **Review logs with `docker-compose logs -f`**
5. **Try complete reset:**
   ```bash
   # Stop everything
   npm run stop:all
   
   # Clean Docker
   cd python-backend
   docker-compose down -v
   docker system prune -f
   
   # Restart
   cd ..
   npm run start:all
   ```

---

## 📚 Additional Resources

- **Python Backend Details:** See `python-backend/README.md`
- **API Documentation:** See `FEATURES.md`
- **Database Setup:** See `SETUP_COMPLETE.md`
- **Project Structure:** See `INTEGRATION_COMPLETE.md`

---

**Happy Building! 🚀** Your NLU ML Platform is ready to create intelligent chatbots!
