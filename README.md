================================================================================
                           RAILSENSE (TRAINSENSE)
    AI-Powered Railway Safety, Telemetry, and Operations Management Platform
================================================================================

1. PROJECT OVERVIEW
-------------------
RailSense (also referred to as TrainSense / RAIL//AI) is an end-to-end,
multi-tiered railway operations, safety, and monitoring platform. It combines
machine learning predictive intelligence, computer vision intrusion detection,
real-time multi-sensor correlation, and role-based operator consoles to prevent
rail conflicts, monitor train telemetry, manage interlocking signals, and provide
passengers with live journey updates.


2. SYSTEM ARCHITECTURE
----------------------
The platform is structured into modular sub-services:

+-----------------------------------------------------------------------------+
|                            USER INTERFACE (FRONTEND)                        |
| React + Vite: Web Consoles for Operators, Passengers, & Network Simulation  |
+------------------------------------+----------------------------------------+
                                     |
             +-----------------------+-----------------------+
             | (REST / JWT Auth)                             | (REST & WebSockets)
             v                                               v
+------------------------------------+    +-----------------------------------+
|     OPERATOR AUTH SERVICE          |    |     CORE TELEMETRY & ML BACKEND   |
| Node.js + Express + MongoDB (RBAC) |    | FastAPI + Python + Scikit/XGBoost |
| - Operator Signup/Login/Session    |    | - Real-time Telemetry Processing  |
| - JWT Generation & Role Checking   |    | - ML Conflict & Delay Prediction  |
| - Port: 5001                       |    | - CV Track Intrusion Detection    |
|                                    |    | - Sensor Correlation Engine       |
|                                    |    | - WebSocket Alert Broadcasting    |
|                                    |    | - Port: 8000                      |
+------------------------------------+    +-----------------------------------+
                                                             |
                                                             v
                                                  +----------------------+
                                                  | PRE-TRAINED ML MODELS|
                                                  | XGBoost JSON Models  |
                                                  | Feature Metadata     |
                                                  +----------------------+


3. CORE MODULES & FEATURES
--------------------------
1. Real-Time Telemetry & Cab Signaling:
   - Dynamic track speed ceiling enforcement.
   - GPS telemetry tracking and train positioning.

2. Machine Learning Conflict & Delay Prediction:
   - Pre-trained XGBoost models evaluate speed, distance, route blockages,
     and environmental conditions to predict conflict probabilities and delays.

3. Computer Vision Track Intrusion Detection:
   - Vision algorithms (YOLO / OpenCV) detect track obstacles, trespassers,
     and foreign objects with real-time bounding box alerts.

4. Correlation Engine & Risk Scoring:
   - Ingests telemetry, ML predictions, and vision detections simultaneously.
   - Calculates unified hazard scores (LOW, MEDIUM, HIGH, CRITICAL) and
     broadcasts actionable emergency triggers.

5. Role-Based Consoles:
   - Loco Pilot DMI: Driver Machine Interface for cab signaling and warnings.
   - Station Master: Track interlocking, route setting, and platform dispatch.
   - Control Room: Bird's-eye regional network operations and dispatching.
   - Simulator: Interactive scenario sandbox for emergency tests & drills.
   - Passenger Portal: Public train tracking, delay alerts, and seat info.


4. DIRECTORY STRUCTURE
----------------------
RailSense/
├── Backend-Auth/           # Node.js + Express + MongoDB authentication service
│   ├── middleware/         # JWT verification and RBAC authorization
│   ├── models/             # Mongoose schemas (User / Operator)
│   ├── routes/             # Auth routes (/api/auth/signup, /login, /me)
│   ├── server.js           # Server entry point
│   ├── package.json        # Node.js dependencies
│   └── .env.example        # Auth environment configuration
│
├── FastApi-backend/        # Python FastAPI core railway logic & ML engine
│   ├── app/
│   │   ├── api/            # REST API endpoints & WebSocket handlers
│   │   ├── core/           # Configuration & settings
│   │   ├── correlation/    # Multi-sensor hazard correlation engine
│   │   ├── event_bus/      # Internal async event publish/subscribe
│   │   ├── ml/             # XGBoost inference & prediction service
│   │   ├── models/         # Pydantic data schemas & internal models
│   │   ├── risk/           # Risk assessment & severity scoring
│   │   ├── services/       # Simulation, trains, and alerts services
│   │   ├── vision/         # Track intrusion CV detection pipeline
│   │   └── main.py         # FastAPI application entry point
│   ├── data/               # Static / seed datasets
│   ├── models/             # Local ML model weights & metadata
│   ├── tests/              # Pytest automated test suite
│   ├── requirements.txt    # Python package dependencies
│   └── .env.example        # FastAPI environment configuration
│
├── Frontend/               # React + Vite web dashboard application
│   ├── src/
│   │   ├── auth/           # AuthContext & ProtectedRoute components
│   │   ├── components/     # UI components (Navbar, Hero, ControlRoom, etc.)
│   │   ├── customer/       # Passenger Portal & journey tracking views
│   │   ├── intrusion/      # Track intrusion UI components
│   │   ├── pages/          # Operator pages (LocoPilot, StationMaster, Simulator)
│   │   ├── services/       # API clients for FastApi & Auth backends
│   │   ├── simulator/      # Network simulator state and control components
│   │   ├── App.jsx         # Main routing and navigation shell
│   │   └── main.jsx        # React DOM mount point
│   ├── package.json        # Frontend dependencies
│   ├── vite.config.js      # Vite build configuration
│   └── .env.example        # Frontend environment configuration
│
├── models/                 # Pre-trained models (conflict_model, delay_model)
├── requirements.txt        # Root Python requirements
└── README.txt              # Project documentation (this file)


5. PREREQUISITES
----------------
- Node.js: v18.0.0 or higher
- npm: v9.0.0 or higher
- Python: v3.9 to v3.12
- MongoDB: Local MongoDB instance or MongoDB Atlas connection string
- Git


6. INSTALLATION & SETUP GUIDE
-----------------------------

--- STEP 1: FastApi Core Backend Setup ---
1. Open a terminal and navigate to the backend directory:
   cd FastApi-backend

2. Create and activate a Python virtual environment:
   # macOS / Linux:
   python3 -m venv venv
   source venv/bin/activate

   # Windows:
   python -m venv venv
   .\venv\Scripts\activate

3. Install required Python packages:
   pip install -r requirements.txt

4. Configure environment variables:
   cp .env.example .env

5. Start the FastAPI development server:
   uvicorn app.main:app --reload --port 8000

   Backend will be accessible at: http://localhost:8000
   Interactive Swagger docs at:   http://localhost:8000/docs


--- STEP 2: Node.js Auth Microservice Setup ---
1. Open a second terminal and navigate to the Auth directory:
   cd Backend-Auth

2. Install Node.js dependencies:
   npm install

3. Configure environment variables:
   cp .env.example .env
   # Set MONGO_URI and JWT_SECRET

4. Start the Auth service:
   npm run dev

   Auth service will run at: http://localhost:5001


--- STEP 3: React Frontend Setup ---
1. Open a third terminal and navigate to the Frontend directory:
   cd Frontend

2. Install frontend dependencies:
   npm install

3. Configure environment variables:
   cp .env.example .env

4. Start the Vite development server:
   npm run dev

   Web App will run at: http://localhost:5173


7. OPERATOR ROLES & ACCESS CONTROL
----------------------------------
The platform enforces role-based access control (RBAC):

| Role             | Accessible Consoles                   | Description                             |
|------------------|---------------------------------------|-----------------------------------------|
| LOCO_PILOT       | /loco-pilot, /simulator               | Cab signaling & speed ceiling enforcement|
| STATION_MASTER   | /station-master, /simulator           | Interlocking, route setting, platforms  |
| CONTROL_ROOM     | /control-room, /simulator             | Regional dispatch & incident management |
| ADMIN            | All Consoles + Management             | Universal clearance                     |
| PASSENGER        | /passenger (Public, No login required)| Train schedules, status, and alerts     |


8. API ENDPOINT REFERENCE
-------------------------

FastAPI Core Service (Port 8000):
- GET    /health                     -> Health check status
- GET    /trains                     -> List active trains and current telemetry
- GET    /alerts                     -> List generated risk and conflict alerts
- POST   /alerts/{id}/acknowledge    -> Acknowledge an active hazard alert
- GET    /dashboard                  -> Aggregated metrics & operational summary
- POST   /simulation/trigger-conflict-> Trigger live simulated conflict scenario
- WS     /ws                         -> Real-time telemetry & alert WebSocket stream

Node.js Auth Service (Port 5001):
- POST   /api/auth/signup            -> Register new operator account
- POST   /api/auth/login             -> Login and receive JWT Bearer token
- POST   /api/auth/logout            -> Invalidate session
- GET    /api/auth/me                -> Get profile and role of authenticated operator
- GET    /api/health                 -> Auth service health check


9. SIMULATION & TESTING
-----------------------
To test the complete end-to-end pipeline:

1. Ensure all 3 services are running (FastAPI on 8000, Auth on 5001, Frontend on 5173).
2. Trigger a simulated conflict via curl:
   curl -X POST http://localhost:8000/simulation/trigger-conflict

3. Verify alerts in the dashboard:
   curl http://localhost:8000/alerts
   curl http://localhost:8000/dashboard

4. Run automated test suite:
   cd FastApi-backend
   pytest


10. LICENSE & CREDITS
---------------------
Developed for intelligent railway safety, scheduling, and risk mitigation.
================================================================================
