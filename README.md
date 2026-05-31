<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&height=190&text=TOCE%20Attendance%20Management&fontAlign=50&fontAlignY=38&fontSize=38&fontColor=ffffff&desc=Smart%20Attendance%20Tracking%20with%20a%203D%20Dashboard&descAlign=50&descAlignY=62&color=gradient&customColorList=6,12,20,24,30" alt="TOCE Attendance Management System Banner">
</p>

<h1 align="center">TOCE Attendance Management System</h1>

<p align="center">
  A full-stack web application for managing college attendance, users, subjects, sessions, and reports from one secure 3D dashboard.
</p>

<p align="center">
  <img alt="Python" src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white">
  <img alt="Flask" src="https://img.shields.io/badge/Flask-111827?style=for-the-badge&logo=flask&logoColor=white">
  <img alt="SQLite" src="https://img.shields.io/badge/SQLite-0EA5E9?style=for-the-badge&logo=sqlite&logoColor=white">
  <img alt="JavaScript" src="https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=111827">
  <img alt="Status" src="https://img.shields.io/badge/Status-Active-22C55E?style=for-the-badge">
</p>

<p align="center">
  <a href="#project-overview">Overview</a> |
  <a href="#key-features">Features</a> |
  <a href="#comprehensive-architecture-design">Architecture</a> |
  <a href="#setup-instructions">Setup</a> |
  <a href="#api-highlights">API</a>
</p>

---

## Project Snapshot

| Category | Details |
| --- | --- |
| Project Name | TOCE Attendance Management System |
| Purpose | Digital attendance tracking for college departments |
| Users | Admin, HOD, Teacher, Student |
| Main Value | Faster attendance marking, organized records, and clear reports |
| Frontend Style | Dark 3D glassmorphism dashboard with colorful highlights |
| Backend Style | Flask monolithic architecture with API-driven dashboard actions |

The project is built for TOCE-style academic workflows where administrators, HODs, teachers, and students need a simple way to manage attendance records and view attendance performance.

## Project Overview

TOCE Attendance Management System is a Flask-based attendance platform with a modern 3D glass-style frontend. It allows role-based users to log in, manage departments and users, add students and subjects, take attendance, and generate reports.

The system is designed to reduce manual attendance work, improve record accuracy, and make attendance data easier to search, update, and analyze.

## Key Features

| Feature Area | Highlights |
| --- | --- |
| Authentication | Secure login, forgot password, edit password, password visibility controls |
| User Management | Add, edit, search, and delete users with role-based restrictions |
| Student Management | Store roll number, department, semester, section, batch year, and linked login account |
| Subject Management | Manage subject code, name, department, semester, credits, and assigned teacher |
| Attendance | Create sessions and mark students as present, absent, or leave |
| Reports | Student-wise, subject-wise, and department-wise attendance summaries |
| Dashboard | Role-based cards, management panels, search, filters, and quick actions |
| Design | Responsive dark interface with colorful 3D glass-style visual elements |

## Tech Stack

| Area | Technology |
| --- | --- |
| Backend | Python, Flask |
| Database | SQLite |
| Frontend | HTML, CSS, JavaScript |
| Authentication | Flask sessions |
| Password Security | PBKDF2-HMAC-SHA256 hashing |
| Public Sharing | Optional ngrok tunnel |

## Visual Design System

| Design Element | Style |
| --- | --- |
| Theme | Dark premium dashboard |
| Visual Direction | 3D glassmorphism with glow accents |
| Primary Colors | Violet, blue, emerald, cyan |
| Background | Deep charcoal with soft gradients |
| Components | Rounded panels, glowing buttons, compact data cards |
| Experience Goal | Modern, attractive, readable, and practical for daily college use |

## Comprehensive Architecture Design

The system follows a simple full-stack monolithic architecture. Flask handles page rendering, API routing, authentication, authorization, business logic, and SQLite database access. The frontend uses HTML, CSS, and JavaScript to create an interactive dashboard experience.

### High-Level Architecture

```mermaid
flowchart TD
    A["User Browser"] --> B["Flask Web Server"]
    B --> C["HTML Templates"]
    C --> D["Frontend JavaScript"]
    D --> E["REST-style Flask API Routes"]
    E --> F["Authentication and Role Checks"]
    F --> G["Application Logic"]
    G --> H["SQLite Database"]
    H --> G
    G --> E
    E --> D
    D --> C
```

### Architecture Layers

| Layer | Responsibility | Main Files |
| --- | --- | --- |
| Presentation Layer | Login page, dashboard UI, password pages, responsive 3D design | `templates/*.html` |
| Client Logic Layer | Form handling, search, filtering, modal actions, API calls | JavaScript inside `dashboard.html`, `login.html`, `password_access.html` |
| API Layer | JSON endpoints for users, students, subjects, sessions, attendance, and reports | `app.py` |
| Security Layer | Login, sessions, password hashing, role-based access control | `app.py` |
| Business Logic Layer | Attendance rules, user updates, student records, report calculations | `app.py` |
| Data Layer | Tables, relationships, constraints, seeded departments and default users | `toce.db`, schema inside `app.py` |

### Request Flow

```mermaid
sequenceDiagram
    participant U as User
    participant UI as Browser UI
    participant API as Flask API
    participant AUTH as Auth Logic
    participant DB as SQLite DB

    U->>UI: Enter login details
    UI->>API: POST /api/login
    API->>DB: Find active user by username
    DB-->>API: User record
    API->>AUTH: Verify password hash
    AUTH-->>API: Valid or invalid
    API-->>UI: Session created and user role returned
    UI->>API: Load dashboard data
    API->>AUTH: Check logged-in user and role
    API->>DB: Fetch allowed records
    DB-->>API: Departments, users, subjects, sessions, reports
    API-->>UI: JSON response
    UI-->>U: Render dashboard
```

### Database Architecture

```mermaid
erDiagram
    DEPARTMENTS ||--o{ USERS : contains
    DEPARTMENTS ||--o{ STUDENTS : contains
    DEPARTMENTS ||--o{ SUBJECTS : offers
    USERS ||--o| STUDENTS : may_be
    USERS ||--o{ SUBJECTS : teaches
    SUBJECTS ||--o{ ATTENDANCE_SESSIONS : has
    USERS ||--o{ ATTENDANCE_SESSIONS : creates
    ATTENDANCE_SESSIONS ||--o{ ATTENDANCE_RECORDS : contains
    STUDENTS ||--o{ ATTENDANCE_RECORDS : receives

    DEPARTMENTS {
        integer id PK
        text code
        text name
        text degree
    }

    USERS {
        integer id PK
        text username
        text password_hash
        text role
        text full_name
        text email
        integer department_id FK
        integer is_active
    }

    STUDENTS {
        integer id PK
        integer user_id FK
        text roll_number
        integer department_id FK
        integer semester
        text section
        integer batch_year
    }

    SUBJECTS {
        integer id PK
        text code
        text name
        integer department_id FK
        integer semester
        integer credits
        integer teacher_id FK
        integer is_active
    }

    ATTENDANCE_SESSIONS {
        integer id PK
        integer subject_id FK
        integer teacher_id FK
        text session_date
        text session_type
        text topic
    }

    ATTENDANCE_RECORDS {
        integer id PK
        integer session_id FK
        integer student_id FK
        text status
        text remarks
    }
```

### Role-Based Access Design

```mermaid
flowchart LR
    A["Login"] --> B["Create Flask Session"]
    B --> C{"User Role"}
    C --> D["Admin"]
    C --> E["HOD"]
    C --> F["Teacher"]
    C --> G["Student"]

    D --> H["Full system access"]
    E --> I["Department-level management"]
    F --> J["Attendance and assigned academic data"]
    G --> K["Own attendance report"]
```

| Role | Access Scope |
| --- | --- |
| Admin | Can manage all departments, users, students, subjects, attendance sessions, and reports |
| HOD | Can manage users, students, subjects, and reports mainly for their department |
| Teacher | Can take attendance and view academic records allowed by the system |
| Student | Can view their own profile and attendance report |

### Functional Module Design

| Module | Main Purpose |
| --- | --- |
| Authentication Module | Handles login, logout, forgot password, and change password |
| User Module | Creates, edits, searches, and deletes users based on role permissions |
| Student Module | Stores student academic details such as roll number, semester, section, and batch |
| Department Module | Stores department code, name, and degree details |
| Subject Module | Connects subjects with departments, semesters, credits, and teachers |
| Attendance Module | Creates attendance sessions and saves student attendance status |
| Report Module | Generates student-wise, subject-wise, and department-wise attendance summaries |
| Dashboard Module | Shows statistics, navigation, management panels, and attendance workflow |

### Attendance Workflow

```mermaid
flowchart TD
    A["Select Subject"] --> B["Select Date"]
    B --> C["Select Session Type"]
    C --> D["Load Eligible Students"]
    D --> E["Mark Present, Absent, or Leave"]
    E --> F["Create or Find Attendance Session"]
    F --> G["Save Attendance Records"]
    G --> H["Update Reports and Dashboard Stats"]
```

### Deployment Architecture

```mermaid
flowchart TD
    A["Browser"] --> B["Flask App on Localhost or Server"]
    B --> C["Templates and Static UI"]
    B --> D["API Routes"]
    D --> E["SQLite Database File"]
    B --> F["Optional ngrok Public URL"]
```

For local development, the app runs on `http://localhost:5000`. For temporary public sharing, ngrok can expose the local Flask server through a public HTTPS URL.

### Design Decisions

- A monolithic Flask app keeps the project simple for academic development and demonstration.
- SQLite is used because it is lightweight, file-based, and easy to run without a separate database server.
- API-driven dashboard actions make the frontend more interactive without reloading the whole page.
- Passwords are never stored directly; only salted password hashes are saved.
- Role-based decorators protect sensitive backend routes.
- The database schema uses foreign keys to connect departments, users, students, subjects, sessions, and records.

### Future Scalable Architecture

For a production-level version, the project can be upgraded with:

- PostgreSQL or MySQL instead of SQLite
- Flask Blueprints to separate modules
- Alembic migrations for database version control
- Separate static files for CSS and JavaScript
- REST API documentation using Swagger or OpenAPI
- Docker-based deployment
- Cloud hosting on Render, Railway, AWS, or Azure
- Background jobs for email alerts and report exports

## Project Structure

```text
ATTENDENCE SYSTEM/
|-- app.py
|-- requirements.txt
|-- start.bat
|-- start.sh
|-- toce.db
|-- templates/
|   |-- login.html
|   |-- dashboard.html
|   |-- password_access.html
|   |-- error.html
|-- README.md
```

## Main Pages

- `/login` - Secure login page
- `/forgot-password` - Reset password using username and email
- `/change-password` - Change password after login
- `/dashboard` - Main attendance management dashboard

## Default Login Accounts

| Username | Password | Role |
| --- | --- | --- |
| admin | admin@toce123 | Admin |
| hod_cse | hod@toce123 | HOD |
| teacher1 | teacher@toce123 | Teacher |

Note: If the password was changed from the dashboard, use the updated password.

## Role Permissions

| Feature | Admin | HOD | Teacher | Student |
| --- | --- | --- | --- | --- |
| View dashboard | Yes | Yes | Yes | Yes |
| Manage departments | Yes | Limited | No | No |
| Manage users | Yes | Department users | No | No |
| Manage students | Yes | Department students | Department students | No |
| Manage subjects | Yes | Yes | Limited | No |
| Take attendance | Yes | Yes | Yes | No |
| View reports | Yes | Yes | Yes | Own report |
| Change password | Yes | Yes | Yes | Yes |

## Setup Instructions

### 1. Install Python

Use Python 3.9 or higher.

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Run the App

```bash
python app.py
```

The app starts at:

```text
http://localhost:5000
```

Open the website through the Flask server URL. Do not open the template files directly with `file:///`, because the dashboard needs Flask API routes to load data.

## API Highlights

| Method | Endpoint | Purpose |
| --- | --- | --- |
| POST | `/api/login` | Login user |
| GET | `/api/me` | Get current user |
| POST | `/api/forgot-password` | Reset password |
| POST | `/api/change-password` | Change password |
| GET/POST | `/api/users` | List or create users |
| PUT/DELETE | `/api/users/<id>` | Update or delete user |
| GET/POST | `/api/subjects` | List or create subjects |
| GET/POST | `/api/sessions` | List or create attendance sessions |
| GET/POST | `/api/attendance/<session_id>` | View or save attendance |
| GET | `/api/dashboard/stats` | Dashboard statistics |

## Website Description

This website helps colleges digitize attendance management. Instead of maintaining paper registers or separate spreadsheets, teachers can create sessions, select subjects, mark student attendance, and save records directly in the system.

Admins and HODs can manage users, students, departments, and subjects. Reports make it easier to track attendance performance by student, subject, or department. The updated frontend gives the system a modern 3D dashboard look while keeping the workflow simple and practical.

## Security Notes

- Passwords are stored as salted hashes.
- Login state is handled using Flask sessions.
- API routes are protected with role-based checks.
- SQL queries use parameterized statements.

For production deployment, set a strong fixed Flask secret key, use HTTPS, keep debug mode off, and protect the database file.

## Future Improvements

- Export attendance reports as PDF or Excel
- Add email notifications for low attendance
- Add student self-service report dashboard
- Add charts for monthly attendance trends
- Add deployment configuration for Render, Railway, or PythonAnywhere

## License

This project is created for academic and learning purposes. You can modify and extend it based on your college requirements.
