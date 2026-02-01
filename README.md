# 🛡️ Vigilant Guard - Insider Threat Detection System

A comprehensive MERN stack application for real-time insider threat detection through behavioral analysis, geographic anomaly detection, and bulk download monitoring.

## 🎭 **NEW: Dual Frontend Architecture**

Vigilant Guard now features **TWO separate frontends** for comprehensive demonstration:

- **🛡️ Security Dashboard** (Port 3000) - For security teams to monitor and respond to threats
- **👤 Employee Simulation Portal** (Port 3001) - For simulating threats and demonstrating real-time synchronization

**[📖 Read the Dual Frontend Guide →](DUAL_FRONTEND_GUIDE.md)**

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Architecture](#architecture)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [API Documentation](#api-documentation)
- [Detection Rules](#detection-rules)
- [Technologies](#technologies)

---

## 🎯 Overview

**Vigilant Guard** is an intelligent security monitoring system that detects insider threats through three primary detection mechanisms:

1. **Login/Logout Detection** - Monitors login patterns, failed attempts, and odd-hour access
2. **Bulk Download Detection** - Tracks file downloads across monitored directories
3. **Geographic Anomaly Detection** - Identifies impossible travel and suspicious location changes

The system features a real-time dashboard with WebSocket updates, risk scoring (0-100), and comprehensive alert management.

---

## ✨ Features

### Core Capabilities

- ✅ **Dual Login System** - Secondary security authentication layer
- 🔍 **Real-time Monitoring** - Live threat detection and dashboard updates
- 📊 **Risk Scoring** - Intelligent 0-100 point risk assessment
- 🌍 **Geographic Tracking** - IP-based location analysis
- 📦 **File Activity Monitoring** - Watches Downloads, Desktop, Documents folders
- 🚨 **Alert Management** - Mark threats as solved/unsolved
- 📈 **Analytics Dashboard** - Interactive charts and statistics
- 🔔 **Real-time Notifications** - Instant alert notifications via WebSocket

### Privacy Features

- ❌ No file content reading
- 🔒 Anonymous employee tokens
- ⏰ Auto-logout after 30 minutes inactivity
- 🗑️ Automatic log deletion after 30 days

---

## 🏗️ Architecture

### Technology Stack

**Backend:**

- Node.js + Express.js
- MongoDB with Mongoose
- Socket.IO for real-time updates
- Chokidar for file monitoring
- JWT authentication

**Frontend (Dual Architecture):**

- React.js with Hooks
- Styled Components
- Chart.js for visualizations
- Socket.IO client
- Axios for API calls
- **Security Dashboard** (Port 3000) - Monitoring interface
- **Employee Portal** (Port 3001) - Simulation interface

### Database Collections

1. **LoginActivity** - Login/logout records with risk levels
2. **BulkDownloadAlert** - File download activity alerts
3. **GeographicAlert** - Location-based anomalies
4. **EmployeePattern** - Employee baseline patterns
5. **ActiveThreat** - Consolidated threat dashboard

---

## 🚀 Installation

### Prerequisites

- Node.js (v16 or higher)
- MongoDB (v5 or higher)
- npm or yarn

### Step 1: Clone the Repository

```bash
git clone <repository-url>
cd vigilantGuard
```

### Step 2: Backend Setup

```bash
cd backend

# Install dependencies
npm install

# Create .env file (use .env.example as template)
cp .env.example .env

# Update MongoDB URI in .env
# MONGODB_URI=mongodb://localhost:27017/threat_detection

# Seed database with sample data
npm run seed

# Start backend server
npm run dev
```

Backend will run on `http://localhost:5000`

### Step 3: Frontend Setup

```bash
cd ../frontend

# Install dependencies
npm install

# Start React development server
npm start
```

Frontend will run on `http://localhost:3000`

### Step 4: Install Employee Portal

```bash
cd employee-portal
npm install
```

### Step 5: Run the Application

**Option 1: Run Everything Together (Recommended)**

```bash
# From project root
npm run dev
```

This starts:

- Backend API (Port 5000)
- Security Dashboard (Port 3000)
- Employee Portal (Port 3001)

**Option 2: Run Individually**

```bash
# Backend only
npm run start-backend

# Security Dashboard only
npm run start-security-dashboard

# Employee Portal only
npm run start-employee-portal
```

### Step 6: Access the Applications

1. **Security Dashboard**: `http://localhost:3000`

   - Login with: `EMP001` (no password needed)
   - Monitor threats, view analytics

2. **Employee Portal**: `http://localhost:3001`
   - No login required
   - Simulate threats in real-time
   - Watch updates appear on Security Dashboard

**💡 Tip:** Open both in separate browser windows side-by-side for best demo experience!

---

## ⚙️ Configuration

### Backend Environment Variables (.env)

```env
# Database
MONGODB_URI=mongodb://localhost:27017/threat_detection

# Server
PORT=5000
NODE_ENV=development

# Security
JWT_SECRET=your_secret_key_here

# API Configuration
IP_API_URL=http://ip-api.com/json

# Monitoring Settings
SESSION_TIMEOUT_MINUTES=30
FILE_SCAN_INTERVAL_SECONDS=30
ALERT_RETENTION_DAYS=30

# Frontend URL
CLIENT_URL=http://localhost:3000
```

### Frontend Environment Variables (.env)

```env
REACT_APP_API_URL=http://localhost:5000/api
REACT_APP_SOCKET_URL=http://localhost:5000
```

---

## 📖 Usage

### 1. Employee Registration

Register new employees via API:

```bash
POST /api/auth/register
Content-Type: application/json

{
  "emp_name": "John Doe",
  "emp_id": "E001",
  "country": "United States",
  "city": "New York",
  "usual_login_time": "09:00",
  "usual_logout_time": "17:00"
}
```

### 2. Employee Login

Employees login through the dashboard:

```bash
POST /api/auth/login
Content-Type: application/json

{
  "employee_token": "EMP001",
  "password": "optional"
}
```

### 3. Monitor Dashboard

The dashboard automatically displays:

- Active users count
- Active/solved threats
- Recent alerts table
- High-risk employees
- Alert distribution charts

### 4. Manage Alerts

- **View Details**: Click on any alert in the table
- **Mark as Solved**: Click "Mark Solved" button
- **Filter Alerts**: Use filter buttons (All, Login, Geographic, Bulk)

---

## 📡 API Documentation

### Authentication Endpoints

#### Register Employee

```
POST /api/auth/register
Body: { emp_name, emp_id, country, city }
Response: { success, employee_token, employee }
```

#### Login

```
POST /api/auth/login
Body: { employee_token, password, ip_address }
Response: { success, token, employee_token, employee_name }
```

#### Logout

```
POST /api/auth/logout
Body: { employee_token }
Response: { success }
```

### Dashboard Endpoints

#### Get Statistics

```
GET /api/dashboard/stats
Response: { stats, alertDistribution }
```

#### Get High-Risk Employees

```
GET /api/dashboard/high-risk-employees?limit=5
Response: { employees }
```

### Alert Endpoints

#### Get All Alerts

```
GET /api/alerts?solved=N&alert_type=login&limit=50
Response: { alerts, count, total }
```

#### Solve Alert

```
PUT /api/alerts/:id/solve
Response: { success, alert }
```

### Geographic Endpoints

#### Get Geographic Overview

```
GET /api/geographic/overview
Response: { overview }
```

#### Verify Geographic Alert

```
PUT /api/geographic/:id/verify
Body: { verified: "Yes" | "No" }
Response: { success, alert }
```

### Employee Endpoints

#### Get Active Employees

```
GET /api/employees/active
Response: { employees, count }
```

#### Get Employee Details

```
GET /api/employees/:token
Response: { employee, threatStats, recentLogins }
```

---

## 🔍 Detection Rules

### Login Detection Rules

| Condition            | Risk Score | Risk Level |
| -------------------- | ---------- | ---------- |
| 3-5 failed attempts  | +25        | Medium     |
| 6-10 failed attempts | +35        | High       |
| 10+ failed attempts  | +50        | Critical   |
| Login 10PM-6AM       | +20        | Medium     |
| Login 1AM-3AM        | +30        | Critical   |
| Failed then success  | +40        | High       |

### Bulk Download Rules

| Condition          | Risk Score | Risk Level |
| ------------------ | ---------- | ---------- |
| 30-50 files        | +20        | Medium     |
| 50+ files          | +30        | High       |
| 200-500MB          | +25        | Medium     |
| 500MB-1GB          | +30        | High       |
| 1GB+               | +40        | Critical   |
| Odd hours download | +20        | Medium     |

### Geographic Rules

| Condition                | Risk Score | Risk Level |
| ------------------------ | ---------- | ---------- |
| Impossible travel        | +60        | Critical   |
| High-risk country        | +50        | Critical   |
| New country              | +30        | Medium     |
| Unusual city             | +15        | Low        |
| Odd hours + new location | +25        | Medium     |

### Risk Level Thresholds

- **Low**: 0-24 points
- **Medium**: 25-49 points
- **High**: 50-74 points
- **Critical**: 75-100 points

---

## 🛠️ Technologies

### Backend

- **Express.js** - Web framework
- **Mongoose** - MongoDB ODM
- **Socket.IO** - Real-time communication
- **Chokidar** - File system monitoring
- **JWT** - Authentication
- **Helmet** - Security headers
- **Winston** - Logging
- **Axios** - HTTP client

### Frontend

- **React** - UI library
- **React Router** - Routing
- **Styled Components** - CSS-in-JS
- **Chart.js** - Data visualization
- **Socket.IO Client** - WebSocket client
- **Axios** - API requests
- **React Toastify** - Notifications
- **Moment.js** - Date handling

---

## 📁 Project Structure

```
vigilantGuard/
├── backend/
│   ├── config/
│   │   └── database.js           # MongoDB connection
│   ├── models/                   # Mongoose schemas
│   │   ├── ActiveThreat.js
│   │   ├── BulkDownloadAlert.js
│   │   ├── EmployeePattern.js
│   │   ├── GeographicAlert.js
│   │   └── LoginActivity.js
│   ├── routes/                   # API routes
│   │   ├── alertRoutes.js
│   │   ├── authRoutes.js
│   │   ├── dashboardRoutes.js
│   │   ├── employeeRoutes.js
│   │   └── geographicRoutes.js
│   ├── services/                 # Business logic
│   │   ├── authService.js
│   │   ├── folderMonitoringService.js
│   │   └── locationService.js
│   ├── middleware/               # Express middleware
│   │   ├── auth.js
│   │   └── errorHandler.js
│   ├── utils/                    # Utilities
│   │   ├── ruleEngine.js         # Risk scoring logic
│   │   └── seedData.js           # Database seeder
│   ├── server.js                 # Express app entry
│   ├── package.json
│   └── .env
│
├── frontend/
│   ├── public/
│   │   └── index.html
│   ├── src/
│   │   ├── components/           # React components
│   │   │   ├── AlertDistribution.js
│   │   │   ├── AlertsTable.js
│   │   │   ├── Header.js
│   │   │   ├── HighRiskEmployees.js
│   │   │   ├── PrivateRoute.js
│   │   │   └── StatsCards.js
│   │   ├── context/              # React Context
│   │   │   ├── AuthContext.js
│   │   │   └── DashboardContext.js
│   │   ├── pages/                # Page components
│   │   │   ├── Dashboard.js
│   │   │   └── Login.js
│   │   ├── services/             # API services
│   │   │   ├── api.js
│   │   │   └── socket.js
│   │   ├── styles/
│   │   │   └── global.css
│   │   ├── App.js
│   │   └── index.js
│   ├── package.json
│   └── .env
│
└── README.md
```

---

## 🔐 Security Considerations

1. **JWT Tokens**: Change `JWT_SECRET` in production
2. **MongoDB**: Use authentication in production
3. **CORS**: Configure allowed origins properly
4. **HTTPS**: Use SSL/TLS in production
5. **Rate Limiting**: Implement rate limiting for APIs
6. **Input Validation**: All inputs are validated
7. **Privacy**: No sensitive data is stored

---

## 🧪 Testing

### Run Backend Tests

```bash
cd backend
npm test
```

### Test API Endpoints

```bash
# Health check
curl http://localhost:5000/api/health

# Get dashboard stats
curl http://localhost:5000/api/dashboard/stats
```

---

## 📝 Development

### Start Development Servers

**Terminal 1 - MongoDB:**

```bash
mongod
```

**Terminal 2 - Backend:**

```bash
cd backend
npm run dev
```

**Terminal 3 - Frontend:**

```bash
cd frontend
npm start
```

### Seed Sample Data

```bash
cd backend
npm run seed
```

This creates 5 sample employees (EMP001-EMP005) with login activities.

---

## 🚀 Deployment

### Production Build

**Backend:**

```bash
cd backend
npm start
```

**Frontend:**

```bash
cd frontend
npm run build
# Serve the build folder with a static server
```

### Environment Variables

Update all environment variables for production:

- Use production MongoDB URI
- Set strong JWT_SECRET
- Configure proper CLIENT_URL
- Set NODE_ENV=production

---

## 🐛 Troubleshooting

### MongoDB Connection Failed

- Ensure MongoDB is running: `mongod`
- Check MongoDB URI in .env
- Verify MongoDB port (default: 27017)

### Port Already in Use

- Backend: Change PORT in backend/.env
- Frontend: Set PORT environment variable

### WebSocket Not Connecting

- Verify REACT_APP_SOCKET_URL in frontend/.env
- Check CORS configuration in backend

### File Monitoring Not Working

- Ensure monitored folders exist (Downloads, Desktop, Documents)
- Check file permissions
- Verify Chokidar is installed

---

## 📄 License

MIT License - See LICENSE file for details

---

## 👥 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Open a Pull Request

---

## 📞 Support

For issues and questions:

- Open an issue on GitHub
- Check existing documentation
- Review API documentation

---

## 🎉 Acknowledgments

- IP location data provided by ip-api.com
- Built with the MERN stack
- Inspired by real-world security monitoring needs


