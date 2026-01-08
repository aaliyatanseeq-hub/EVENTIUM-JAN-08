# Event Intelligence Platform

A comprehensive event discovery and management platform that aggregates events from multiple sources including Twitter, Reddit, Ticketmaster, and more.

## 📋 Prerequisites

Before you begin, ensure you have the following installed:

- **Python 3.10+** (check with `python --version`)
- **PostgreSQL 12+** (download from [postgresql.org](https://www.postgresql.org/download/))
- **Git** (for cloning the repository)
- **PowerShell** (Windows) or **Bash** (Linux/Mac)

## 🚀 Quick Start Guide

### Step 1: Clone the Repository

```bash
git clone https://github.com/aaliyatanseeq-hub/EVENTIUM-JAN-08.git
cd EVENTIUM-JAN-08
```

### Step 2: Set Up Python Virtual Environment

**Windows (PowerShell):**
```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

**Linux/Mac:**
```bash
python3 -m venv venv
source venv/bin/activate
```

### Step 3: Install Dependencies

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

**Note:** This may take a few minutes as it installs ML libraries (sentence-transformers, scikit-learn) and other dependencies.

### Step 4: Set Up PostgreSQL Database

1. **Install PostgreSQL** if you haven't already
2. **Create the database and user:**

```sql
-- Connect to PostgreSQL as superuser
psql -U postgres

-- Create database
CREATE DATABASE event_intelligence;

-- Create user (optional, or use existing user)
CREATE USER event_user WITH PASSWORD 'event_password123';

-- Grant privileges
GRANT ALL PRIVILEGES ON DATABASE event_intelligence TO event_user;

-- Connect to the new database
\c event_intelligence

-- Grant schema privileges
GRANT ALL ON SCHEMA public TO event_user;
```

### Step 5: Configure Environment Variables

Create a `.env` file in the `Backend` directory:

```powershell
cd Backend
New-Item -ItemType File -Name .env
```

Add the following configuration to `Backend/.env`:

```env
# Database Configuration (REQUIRED)
DATABASE_URL=postgresql://event_user:event_password123@localhost:5432/event_intelligence

# Twitter API Credentials (OPTIONAL - for Twitter features)
TWITTER_API_KEY=your_twitter_api_key
TWITTER_API_SECRET=your_twitter_api_secret
TWITTER_ACCESS_TOKEN=your_twitter_access_token
TWITTER_ACCESS_TOKEN_SECRET=your_twitter_access_token_secret
TWITTER_BEARER_TOKEN=your_twitter_bearer_token
TWITTER_OAUTH2_ACCESS_TOKEN=your_oauth2_token

# Reddit API Credentials (OPTIONAL - for Reddit features)
REDDIT_CLIENT_ID=your_reddit_client_id
REDDIT_CLIENT_SECRET=your_reddit_client_secret
REDDIT_USERNAME=your_reddit_username
REDDIT_PASSWORD=your_reddit_password
REDDIT_USER_AGENT=EventIntelPlatform/1.0 by u/yourusername

# External API Keys (OPTIONAL)
TICKETMASTER_API_KEY=your_ticketmaster_key
SERP_API_KEY=your_serp_api_key
PREDICTHQ_API_KEY=your_predicthq_key
SPRINGBEE_API_KEY=your_springbee_key

# Redis (OPTIONAL - for caching)
REDIS_URL=redis://localhost:6379
```

**Important Notes:**
- Replace all placeholder values with your actual API keys
- Remove quotes around values (e.g., use `key=value` not `key="value"`)
- No spaces around the `=` sign
- The database URL is **REQUIRED** - the app won't work without it
- API keys are **OPTIONAL** - the app will work but some features may be disabled

### Step 6: Initialize Database Tables

```powershell
cd Backend
python database/init_tables.py
```

This creates all necessary database tables.

### Step 7: Run Database Migration (If Needed)

If you encounter errors about missing columns, run the migration:

```powershell
cd Backend
python database/add_quality_fields.py
```

Or use the migration script:

```powershell
python Backend/run_migration.py
```

### Step 8: Start the Server

**Option 1: Using PowerShell Script (Recommended for Windows)**
```powershell
cd Backend
.\start_server.ps1
```

**Option 2: Manual Start**
```powershell
cd Backend
$env:PYTHONIOENCODING="utf-8"
python -m uvicorn app:app --host 127.0.0.1 --port 8000 --reload
```

**Option 3: Using Batch File**
```cmd
cd Backend
start_server.bat
```

### Step 9: Access the Application

Once the server is running, open your browser and navigate to:

- **Frontend/Web Interface:** http://localhost:8000
- **API Documentation:** http://localhost:8000/docs
- **Alternative API Docs:** http://localhost:8000/redoc

## 📁 Project Structure

```
EVENTIUM-JAN-08/
├── Backend/
│   ├── app.py                 # Main FastAPI application
│   ├── database/              # Database models and CRUD operations
│   ├── engines/               # Event and attendee processing engines
│   ├── services/              # External API clients (Twitter, Reddit, etc.)
│   ├── config/                # Configuration files
│   ├── migrations/            # Database migration scripts
│   └── .env                   # Environment variables (create this)
├── frontend/
│   ├── index.html            # Main frontend page
│   ├── js/app.js             # Frontend JavaScript
│   └── styles/main.css       # Frontend styles
├── requirements.txt          # Python dependencies
└── README.md                 # This file
```

## 🔧 Troubleshooting

### Port 8000 Already in Use

If port 8000 is already in use, you can:
1. Kill the process using the port:
   ```powershell
   .\kill_port_8000.ps1
   ```
2. Or use a different port:
   ```powershell
   python -m uvicorn app:app --host 127.0.0.1 --port 8001 --reload
   ```

### Database Connection Errors

1. **Check PostgreSQL is running:**
   ```powershell
   Get-Service -Name postgresql*
   ```

2. **Verify database exists:**
   ```sql
   psql -U postgres -c "\l"
   ```

3. **Check .env file:**
   - Ensure `DATABASE_URL` is correct
   - No quotes around values
   - No spaces around `=`

### Import Errors

If you get import errors:
1. Make sure virtual environment is activated
2. Reinstall dependencies: `pip install -r requirements.txt`
3. Ensure you're running from the correct directory

### Unicode/Encoding Errors

The startup scripts set `PYTHONIOENCODING=utf-8` automatically. If you run manually, set it:
```powershell
$env:PYTHONIOENCODING="utf-8"
```

## 📚 Additional Documentation

- **HOW_TO_RUN.md** - Detailed server startup instructions
- **RUN_MIGRATION.md** - Database migration guide

## 🎯 Features

- **Multi-Source Event Discovery:** Twitter, Reddit, Ticketmaster, PredictHQ
- **Smart Event Filtering:** Quality scoring and noise filtering
- **Attendee Matching:** Semantic matching for event-attendee relevance
- **RESTful API:** Full FastAPI documentation
- **Web Dashboard:** User-friendly web interface
- **Database Dashboard:** Monitor events and data

## 🔐 Security Notes

- Never commit your `.env` file to version control
- Keep API keys secure and rotate them regularly
- Use strong database passwords in production
- The `.env` file is already in `.gitignore`

## 📝 License

[Add your license information here]

## 🤝 Support

For issues or questions, please open an issue on the GitHub repository.

