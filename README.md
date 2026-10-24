🎓 Smart Placement Management System (SPMS)

An end-to-end, full-stack campus recruitment platform that automates placement drives, student profile management, company directories, mock assessment tests, and ATS-based resume evaluation.

Built with a React frontend, a Flask (Python) REST API backend, and a normalized MySQL (InnoDB) relational database.

📌 Table of Contents
Key Features
System Architecture
Database Design & Schema (DBA Overview)
Tech Stack
Project Directory Structure
Getting Started & Installation
1. Prerequisites
2. Database Setup
3. Backend Setup (Flask API)
4. Frontend Setup (React App)
Default Demo Accounts
API Endpoints Overview
Contributing & License
✨ Key Features
👨‍🎓 For Students
Authentication & RBAC: Secure sign-up/login with JWT authorization.
Profile Management: Maintain academic records (Degree, Department, CGPA, Backlogs, Contact, Resume URL).
Placement Drives: Browse active campus recruitment drives with real-time eligibility checks (CGPA cutoff, max allowed backlogs).
One-Click Application: Apply for eligible drives; duplicates prevented by database constraints.
Application Tracker: Monitor stages in real-time (Applied, Under Review, Shortlisted, Interview, Selected, Rejected).
Mock Tests & Assessments: Practice technical and aptitude quizzes with instant scoring.
ATS Resume Analyzer: AI-assisted resume scanner providing score and skill improvement suggestions.
🏢 For Companies & Recruiters
Company Profiles: Showcase industry, location, website, packages (LPA), and hiring roles.
Drive Scheduling: Publish job vacancies, eligibility criteria, and application deadlines.
Candidate Review: Access verified applicant pools for campus interview shortlisting.
🛡️ For Placement Admins (College TPO)
Central Placement Dashboard: Overview of total students, active drives, total applications, and test statistics.
Student Verification: Verify student profiles and validate eligibility before placement drives commence.
Drive Management: Create, update, or archive recruitment drives.
🏛 System Architecture
+-------------------------------------------------------------+
|                     Client (React 18)                       |
|   Tailwind CSS / Lucide Icons / Axios / React Router Dom    |
+-------------------------------------------------------------+
                              |
                     HTTPS / JSON REST APIs
                              |
+-------------------------------------------------------------+
|                  Backend API (Flask / Python)               |
|      JWT Authentication / CORS / Marshmallow Serialization  |
+-------------------------------------------------------------+
                              |
                      PyMySQL Connection
                              |
+-------------------------------------------------------------+
|                 Database (MySQL 8 - InnoDB)                 |
|   Relational Tables, Foreign Key Constraints, B-Tree Indexes|
+-------------------------------------------------------------+
🗄 Database Design & Schema (DBA Overview)

The database smart_placment_mangement_system is designed in Third Normal Form (3NF) with strict referential integrity.

Entity Relationship Overview
Table	Description	Primary Key	Key Foreign Keys & Constraints
users	Core credentials & roles (student, admin, recruiter)	id	email (UNIQUE, INDEXED)
students	Academic profile, CGPA, backlogs, skills	id	user_id -> users(id) [ON DELETE CASCADE]
companies	Recruiter directory & contact info	id	company_name (UNIQUE)
jobs	Drive listings, cutoffs, and deadlines	id	company_id -> companies(id) [ON DELETE SET NULL]
applications	Student drive applications	id	(student_id, job_id) [UNIQUE COMPOSITE]
tests	Mock test questions & categories	id	questions_json (JSON)
test_results	Assessment scores & submissions	id	student_id -> users(id), test_id -> tests(id)
resume_analyses	ATS scores and extracted keywords	id	student_id -> users(id)
Database Integrity & Performance Features:
Cascading Deletes (ON DELETE CASCADE): Ensures that when a user account is removed, all dependent profile records, applications, and test results are automatically purged.
Duplicate Prevention: A composite UNIQUE (student_id, job_id) constraint guarantees students cannot apply to the same job multiple times.
Targeted Indexing: B-Tree indexes created on high-frequency search fields:
idx_users_email (instant login queries)
idx_jobs_deadline & idx_jobs_status_company (fast drive filtering)
idx_applications_status (rapid shortlist generation)
💻 Tech Stack
Frontend: React 18, React Router v7, Axios, Lucide React, Recharts
Backend: Python 3.11+, Flask 3.1, Flask-JWT-Extended, PyMySQL, python-dotenv
Database: MySQL 8.0+ (InnoDB Engine, utf8mb4_unicode_ci)
Security: PBKDF2/SHA-256 password hashing, JWT Bearer token authentication
📁 Project Directory Structure
text
SPMS/
├── smart_db.sql               # MySQL database schema definition & initial tables
├── run-spms.sh                # Single-command startup script
├── view_students.sh           # Terminal helper to inspect registered students
│
├── SPMS_Backend/              # Flask Backend Service
│   ├── app/
│   │   ├── models/            # Data models and CRUD abstractions
│   │   ├── routes/            # Blueprint API endpoints (auth, jobs, admin, etc.)
│   │   ├── config.py          # Database & JWT configurations
│   │   ├── extensions.py      # Database connector initializer
│   │   └── mysql_db.py        # MySQL database helper & table seeders
│   ├── .env.example           # Template for environment configuration
│   ├── requirements.txt       # Python dependencies
│   ├── run.py                 # Backend application entry point (Port 5001)
│   ├── seed_companies.py      # Pre-populates sample recruitment drives
│   └── migrate_db.py          # Safe database schema updater
│
└── SPMS_Frontend/             # React Frontend Service
    ├── public/                # Static HTML & assets
    ├── src/
    │   ├── components/        # Reusable UI components (Navbar, Sidebar, Layout)
    │   ├── pages/             # View pages (Admin, Student, Auth, Drives)
    │   ├── services/          # Axios HTTP client & API handlers
    │   ├── App.jsx            # Main application router
    │   └── index.js           # React root mounting
    └── package.json           # Frontend dependencies & scripts (Port 3000)
🚀 Getting Started & Installation
1. Prerequisites

Ensure you have the following installed on your machine:

Node.js (v18.x or newer) & npm
Python (v3.10 or newer)
MySQL Server (v8.0 or newer)
2. Database Setup (MySQL)
Open your terminal or MySQL Workbench and log into your MySQL instance:
bash
mysql -u root -p
Import the project schema:
sql
SOURCE /path/to/SPMS/smart_db.sql;
(Or copy-paste the contents of smart_db.sql into MySQL Workbench and run it).
3. Backend Setup (Flask API)
Navigate to the backend directory:
bash
cd SPMS_Backend
Create and activate a Python virtual environment:
bash
# Windows (PowerShell):
python -m venv venv
.\venv\Scripts\Activate.ps1
# macOS / Linux:
python3 -m venv venv
source venv/bin/activate
Install dependencies:
bash
pip install -r requirements.txt
Configure your environment variables:
Copy .env.example to .env:
bash
cp .env.example .env
Update .env with your MySQL root password:
env
MYSQL_HOST=localhost
MYSQL_PORT=3306
MYSQL_USER=root
MYSQL_PASSWORD=your_mysql_password
MYSQL_DB=smart_placment_mangement_system
JWT_SECRET_KEY=spms_super_secret_jwt_key
(Optional) Seed sample company data:
bash
python seed_companies.py
Start the Flask server:
bash
python run.py
The backend will run on http://127.0.0.1:5001.
4. Frontend Setup (React App)
Open a new terminal and navigate to the frontend directory:
bash
cd SPMS_Frontend
Install frontend dependencies:
bash
npm install
Launch the development server:
bash
npm start
The application will automatically open at http://localhost:3000.
🔑 Default Demo Accounts

If you run the initial database scripts, the following demo accounts are available:

👨‍💻 Author & Acknowledgments
Author Name : VIPUL KUMAR YADAV
Project Name: Smart Placement Management System (SPMS)
Role: Database Administrator (DBA) & Full Stack Contributor
Institution: Cochin University of Science and Technology (CUSAT)
📄 License

This project is licensed under the MIT License - feel free to use and adapt it for academic and institutional placement management.
